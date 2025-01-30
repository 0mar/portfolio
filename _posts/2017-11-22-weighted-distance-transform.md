---
title: "Implementation of a weighted distance transform"
layout: post
date: 2017-11-21
hidden: true
tags: [programming, fortran, math, mercurial]
---
{% include mathjax.html %}
I recently finished a Python module for computing weighted or generalized distance transforms (available [here](https://github.com/0mar/weigted-distance-transform)). I needed it for the motion planner in my crowd simulation [Mercurial]({% post_url 2016-12-30-mercurial %}) and I couldn't find it online, so I implemented it in Fortran so F2PY could [wrap]({% post_url 2016-12-15-python-fortran %}) it for me. This post goes into details of the algorithm and how to use the module.

<!--more-->
A [distance transform](https://en.wikipedia.org/wiki/Distance_transform) is a function defined over an image that assigns to each pixel the distance to the nearest boundary.
Apart from motion planning, it is often used in image analysis in applications like (de)blurring and shape recognition. A standard version is implemented in SciPy's `morphology` module.
Unfortunately, that distance transform didn't cut it for me, since it does not allow for specific boundary placement. In addition, standard distance transform are based on a uniform distance metric, and I required a *weighted* distance. In other words, I wanted to specify a non-uniform cost field for each of the pixels.

In my case, a standard distance transform provides for each pixel a distance to the nearest goal. But what if some areas would be less accessible, or require more effort to move through? Computing not the shortest, but the fastest or easiest path to the goal, requires a specific cost field to weigh the path segments with.

I built the implementation based partly on [this](https://www.mit.edu/~jnt/dijkstra.html) and [this](https://grail.cs.washington.edu/projects/crowd-flows/78-treuille.pdf) paper. I was lucky enough to find a suitable Fortran implementation of the heap data structure (thanks [Daniel Pena](https://github.com/trifling/mheap)!), but because of limitations in F2PY I had to backport it from Fortran 2003 to Fortran 95.

## What does it do?

The module exposes three functions:
 * `map_image_to_cost(image)` which opens and reads `image` and convert the image to a cost field based on the implemented mapping.

The default mapping is what I use in Mercurial; it considers red zoned as goals and black zones as obstacles. Accessible space is in white, and less accessible regions can be indicated by using darker shades of grey.
Other mappings can be implemented by overriding this function.

 * `get_weighted_distance transform(cost_field)`, which reads a cost field and uses it to compute a distance transform.

If we denote the cost field with $u(x)$ and the weighted distance transform with $\Phi(x)$, this function returns the solution to the Eikonal equation:

$$ ||\nabla \Phi|| = u(x),$$

where $u(x)=0$ for each $x$ in the goal area.
The Eikonal equation is solved by applying a first order upwind discretization method. Because of the non-standard boundary conditions, the upwind direction is determined for each cell with the fast marching algorithm. Cells are added to the distance transform in order of their distance, similar to Dijkstra's algorithm. As a result, $\Phi$ is continuous everywhere, and only non-smooth where $u$ has jumps.

 * `plot(field)`, a Matplotlib wrapper function for a surface plot.

## Usage

Let's apply the module to the image below (a simple crowd dynamics scenario).

<img src="/assets/wdt_files/funnels.png" alt="funnels.png" style="width: 450px;"/>

We load the image `funnels.png` and convert it to a cost field:
```python
>>> import wdt
>>> cost_field = wdt.map_image_to_costs('funnels.png')
>>> wdt.plot(cost_field)
```
![png](/assets/wdt_files/costs.png)
We compute the distance transform:
```python
>>> distance_transform = wdt.get_weighted_distance_transform(r)
>>> wdt.plot(distance_transform)
```
![png](/assets/wdt_files/wdt.png)
Notice how following the steepest descent of the distance transform always provides the fastest way to the goal.

I hope you'll find the module useful. Let me know if you encounter any bugs!
