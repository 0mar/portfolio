---
title:  "Mercurial: simulation framework for crowd dynamics"
date:   2016-11-30 15:34:13 +0100
tags: [master, graduation project, crowd dynamics, mercurial]
---

Recently, I wrote something about my [graduation project]({% post_url 2016-11-20-graduation-project %}). Part of this project was the development of a simulation framework, *Mercurial*. The goal of *Mercurial* is to facilitate the implementation of different hybrid particle simulation methods, and to test them on simple but existing use cases with different environments. You can find the code on GitHub [here](https://github.com/0mar/mercurial), together with instructions on how to use it.

If you are interested in the structure of the code, check [this]({% post_url 2018-02-05-mercurial-2 %}) post.
This simulation was also used in my research internship at [LOC7000](http://www.loc7000.com/en/), to evaluate evacuation scenarios on Dutch music festival Lowlands and see if the safety measures could be analyzed in a quantitative manner.

The framework is built in Python. The simulation creates an environment from a scene file and reads in the parameters from a configuration file. Alternatively, a simple drawing tool is included to create new environments.
<!--more-->
*Mercurial* is being designed as an simulation of interacting populations that takes advantage of the speed of models posed on multiple scales and runs fast using routines from optimized libraries. While Python as an interpreted language is not particularly fast, most of the overhead can be circumvented by executing computationally intensive work inside of these libraries. For tasks outside of these libraries, we built subroutines in Fortran and compiled them to Python library objects.

As a result, the simulation (currently non-parallel) simulates a crowd of 50000 pedestrians in realtime on a Intel i7 machine, updating every 0.03 seconds.

Simulation scenarios consists of environments with rectangular exits, entrances and obstacles of various size, as shown in the image below.

<img src="/assets/combi_scene.png" alt="Example environment" style="width: 300px;"/>

This image shows a small-scale environment from the Mercurial framework. Entrances are in green, walls in black and exits in red.

<img src="/assets/comp_dynamic.png" alt="Example environment" style="width: 300px;"/>

This image shows an large-scale environment with several obstacles and one exit.

Using the matplotlib and tkinter libraries, we provide simple but efficient overviews of the environment and the crowd, as well as several macroscopic quantities like density, crowd velocity, pressure and discomfort.

<img src="/assets/comp_dynamic_fields.png" alt="Macroscopic quantities" />

This image shows macroscopic quantities density, discomfort and potential field of the last environment.

## Pedestrian wayfinding algorithm

Every crowd dynamics model requires an algorithm that guides the pedestrians towards their goals. In Mercurial, this is done by a (distance-based) potential field planner. This means that the environment is divided into a fine raster, and for every cell in the raster a value is computed that increases with its distance to an exit. Below you can see an example.

<img src="/assets/transport_scene.png" alt="Example environment" style="width: 300px;"/>
*A maze-like environment with the exit on the right side*

<img src="/assets/transport_potential.png" alt="Example environment" style="width: 300px;"/>
*A 3D plot of the potential field defined on the environment*

Interaction is implemented by making certain the number of people in a small area (the pressure) does not exceed a threshold. Regions of pressure are identified by measuring the density \\(\rho\\), the velocity \\(v\\), and solving the partial differential equation

$$\frac{\partial \rho}{\partial t} =- \nabla\cdot {\rho(v-\nabla p)}$$

for \\(p\\). We do so by converting the equation to a linear complementarity problem and using the projected Gauss-Seidel method to find a solution.

We use the pressure to change the crowd velocity accordingly and alter the individual velocities of the pedestrians to follow the tendencies of the crowd.

Below is a video showing the simulation for one of the environments above.

<iframe width="560" height="315" src="https://www.youtube.com/embed/5425NTMoHbA" frameborder="0" allowfullscreen></iframe>

If you are interested in these kinds of simulations, take a look at the program and at the thesis for more details and references to other methods.
