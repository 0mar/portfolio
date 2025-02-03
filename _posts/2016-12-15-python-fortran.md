---
title:  "Extending Python with Fortran"
date:   2016-12-15 15:34:13 +0100
tags: programming python fortran hpc
---
If you use Python for scientific programming you'll probably find that as long as you can use NumPy, SciPy, and other optimized libraries, scripts can be developed and executed swiftly.

The routines from these libraries are often composed in low-level programming languages (like C or assembly), avoiding the overhead from the dynamic and interpreted nature of Python.

However, it can happen that some of the algorithms you are implementing require operations not supported in these libraries. For example: if you use NumPy, but run into an algorithm that is difficult to expressed in a vectorized form, you might have to resort to pure Python. This can become a huge bottleneck in the execution of your script.

It is possible to create your own Python module from a lower-level programming language. This is called [extending](https://docs.python.org/3/extending/extending.html) Python. This means you write the bottleneck algorithm in the low-level language, compile it in a Python-friendly way and use it in your script like any other module. This post shows how to use the tool F2PY from SciPy to create an extension from Fortran.
<!--more-->
## Extending Python with Fortran
Disclaimer: I am biased towards Fortran. In general, because despite its verbose syntax and limited structures (or because?), it gets things done. Fast. In this particular case because extending Python is quite easy, in comparison to extending with C or C++, which requires quite some object wrapping and boilerplate code code.
On the contrary, using F2PY, Fortran is able to directly interface with NumPy arrays, without any conversion or layers in between.

Let's examine how to build your own Python module in Fortran. For this example you need a Unix environment with an installation of Python 2/3, the SciPy stack, and a Fortran compiler (like gfortran).

### Computing a list of prime numbers

The process is illustrated with a simple example: dynamically computing a list of prime numbers.
The nature of this algorithm makes it difficult to express in vectorized operations, but does not require complex data structures. This is the kind of problem where extending with Fortran can make a big difference without a lot of effort.

First I'll demonstrate the algorithm in Python (using NumPy arrays to store the values), then the steps to obtain the Fortran extension:

1. A Fortran implementation of the routine
2. Compiler instructions for F2PY in the implementation
3. F2PY commands to create the Python module.

The Python code snippet:

```python
import numpy as np

n = 5000 # Number of primes to find
primes = np.zeros(n,dtype='int') # List of prime numbers

primes[0] = 2 # First prime
primes_found = 1
prime_cand = 3 # Number that is tested

while primes_found < n:
    ind = 0
    while ind < primes_found:
        if prime_cand % primes[ind] == 0: # Only test division with primes
            break # divisible, no prime
        else:
            ind += 1
    if ind == primes_found:
        primes[primes_found] = prime_cand # not divisible, so new prime
        primes_found += 1
    prime_cand += 1
```

Running this algorithm on my machine takes 5.27 seconds: that's our benchmark.

### Fortran implementation

Now, we program the same routine in Fortran. No special care is required, we only need to define the relevant part of the code in a `subroutine`, which is good practice in any case.

```fortran
program example
implicit none
integer (kind=8), parameter :: n = 5000
integer (kind=8), dimension(0:n-1) :: primes

call get_primes(n,primes)
end program

subroutine get_primes(n,primes)
  implicit none
  integer (kind=8) :: n
  integer (kind=8), dimension(0:n-1) :: primes
  integer (kind=8) :: primes_found, prime_cand, ind

  primes(0) = 2
  primes_found = 1
  prime_cand = 3

  do while (primes_found < n)
    ind = 0
    do while (ind < primes_found)
      if (mod(prime_cand,primes(ind)) == 0) then
        exit
      else
        ind = ind + 1
      end if
    end do
    if (ind == primes_found) then
      primes(primes_found) = prime_cand
      primes_found = primes_found + 1
    end if
    prime_cand = prime_cand + 1
  end do
end subroutine
```

Remember that by default, Fortran uses 1-based indexing. It supports 0-based indexing (as well as any other integer) if you indicate this manually. While in general, I try to stick to the default as much as possible when it comes to programming, I more than once came to regret not adapting the indexing when porting an algorithm.

Using `gfortran` with optimization flag `-O2` to compile the code I clock a runtime of 0.17 seconds.
That's a statement on its own.

### F2PY compiler instructions
The power of F2PY is that without any changes to the Fortran source code, we are able to harness this power into Python. All we need are a few compiler directives (included as Fortran comments), and the `subroutine` is able to natively interface with Python and NumPy arrays.

Compiler instructions are preceded with `!f2py` so they are ignored by the Fortran compiler and interpreted by F2PY. Common instructions are

* `!f2py intent(in) var`: `var` becomes an input argument. This means any modification to `var` in the Fortran code won't return to the Python script it was called from.

* `!f2py intent(out) var`: `var` becomes an output argument. It won't appear in the method signature in Python, but it will be a return value. If you indicate multiple output arguments, the method returns a tuple containing all of them.

* `!f2py intent(inout) var`: `var` becomes both an input and output argument, meaning that you provide it in the method call, and the Fortran script will make any modifications in-place. This can come in handy when you work with large arrays and want to avoid the cost of a copy operation.

* `!f2py intent(number) array`: the size of `array` depends on integer `number` which does not need to be known when the module is compiled. This allows to work with arrays of arbitrary size.

In our code, we need the following instructions:
```fortran
subroutine get_primes(n,primes)
  implicit none
  integer (kind=8) :: n
  integer (kind=8), dimension(0:n-1) :: primes
  integer (kind=8) :: primes_found, prime_cand, ind
!f2py intent(in) n
!f2py intent(out) primes
!f2py depend(n) primes
  ...
```

The subroutine is the part that will be compiled to a Python method. If you want, you can remove the main `program` from the source, but usually I leave it it for testing purposes.

### Compilation of extension module
Save the Fortran code above in a file called `prime_example.f90`.
Creating the Python module is done by running the command

```bash
f2py -c prime_example.f90 -m primemod
```
The `-c` flag indicates the compilation of Fortran source files. The `-m` flag specifies the name of the module.

Note that NumPy ships with both a Python 2 (`f2py`) and a Python 3 (`f2py3`) executable. In both cases, the result is a shared library object that can be imported in a Python script.

### Using the new module
After compilation, we can test our method in a Python session:

```
> import primemod
> print(primemod.get_primes.__doc__)
primes = get_primes(n)

Wrapper for ``get_primes``.

Parameters
----------
n : input int

Returns
-------
primes : rank-1 array('i') with bounds (n)
> primes = primemod.get_primes(5000)
array([    2,     3,     5, ..., 48589, 48593, 48611], dtype=int64)
```

As you can see, the Python method is generated with a nice function documentation. We call the method with an integer, and it returns a proper NumPy array. Splendid!

Finally, to compare it with the benchmark I clocked the new module and obtained a runtime of 0.091 seconds. Faster than the Fortran original because F2PY uses all fortran compiler optimizations it can find.

## Remarks on Fortran versus Python storage
Python (NumPy) stores its arrays in a row-major layout. This means that internally, the arrays are stored row by row.
Fortran on the other hand, stores its arrays in a *column-major* layout; storing arrays column by column.
While generally this has no consequences for your interfacing, there are two things to keep in mind.
1. If you want to use compiler instruction `intent(inout) arr`, the layout of the NumPy array `arr` _must_ be Fortran contiguous. This is possible by initializing the array in your Python script with `arr = np.array(...,order='F')`.
2. Because of Fortran's layout, it is more efficient to loop over rows than over columns. Iterating over a 2D array would look like this:
```fortran
do j=1,m
  do i=1,n
    arr(i,j) = ...
  end do
end do
```

For more details and different examples, look on the [F2PY page](https://docs.scipy.org/doc/numpy-dev/f2py/) itself. Happy wrapping!
