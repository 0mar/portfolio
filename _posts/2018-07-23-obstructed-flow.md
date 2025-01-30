---
layout: post
title:  "Obstructed flow: particle flux through a permeable membrane"
hidden: true
tags: mathematics multiscale fem
---

{% include mathjax.html %}

Imagine a group of ducklings on one end of a corridor. When a bell sounds, all of them will waddle towards the other end of the corridor, as chaotically and unorganised as ducklings usually waddle.
Now imagine that you would like to guide these ducklings by placing objects in the corridor.
Is it possible to strategically introduce obstacles in the corridor in order to reduce the time it takes them to get to the other end?
The answer is: yes, under specific circumstances.

Together with researchers from Sapienza University and Giustino Fortunato University (Rome, Italy), we are investigating models of flow through obstructed spaces. Our most recent investigation has led to [this paper][1], currently submitted for publication.
<!--more-->

The effect of obstacle placement to increase flow has been studied in many different contexts. In this article, we examine the effect of a large number of arbitrarily thin obstacles located in the center of the corridor (see Figure 1). The particles are represented by a continuous flow moving from left to right.

<img src="/assets/membrane_files/geometry.png" />
*Figure 1: Schematic overview of the geometry. The filled bars in the center are impenetrable.*

Using upscaling techniques, we obtain an analytical solution in the case of a near infinite number of obstacles. We see that the vertical diffusion is strongly reduced.
In a system where particles move with a so-called *direction exclusion* (where with each step, a particle can only move in one principle direction), this results in an increased horizontal diffusion. Because of the drift and the presence of other particles, the throughput of the corridor is increased (for a specific observation in the case of one obstacle, see [this paper][2]).

We performed a simulation for various parameters, testing the effects of the obstacles on the diffusion. By deriving the upscaled model and simulating it using a finite element scheme, we recovered the dynamics of this phenomenon.

<img src="/assets/membrane_files/macro.png" />
*Figure 2: Macroscopic stationary distribution of concentrations for aligned obstacles and diffusion.*

When the diffusion axes are aligned with the obstacles, no significant change in transport takes place (see Figure 2). But when these directions change, there is a large impact, even though the obstacles are infinitely small, as you can see in Figure 3.

<img src="/assets/membrane_files/cross_term_macro.png" />
*Figure 2: Macroscopic stationary distribution of concentrations for disaligned obstacles and diffusion.*

By changing the shape of the obstacles, you can tune the resulting diffusion. Figure 4 shows the diffusion for various shape sizes (characterised by shape parameter $\delta$).
There seems to be an optimum with respect to shapes. What needs to be investigated is if this is due to the numerical model or to the properties of the geometry.

<img src="/assets/membrane_files/optimal_shapes.png" />
*Figure 2: Horizontal diffusion with respect to obstacle shape. A minimum is attained for $\delta\approx 0.15$.*

Although it is meant as a simple example, the particles of our investigation match the behaviour of the aforementioned ducklings quite well: there is a force driving them towards the end of the corridor, all the while they experience diffusion in vertical direction, due to the nature of their motion and the bumping into other ducklings.

More generally, this phenomena occurs in all kinds of transport systems where one direction of motion is prevalent, but there are additional forces affecting the agents.
Other examples include people moving through a subway station, the transport of $\textrm{CO}_2$ in a cell, or a school of fish swimming downstream in a river.

One of our future goals is to develop this method further so we can better understand how to change geometries and shape public environments to optimize flow.


[1]: http://arxiv.org/abs/1804.08392
[2]: https://journals.aps.org/pre/abstract/10.1103/PhysRevE.94.042115
