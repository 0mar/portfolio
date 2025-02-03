---
title:  "Graduation project: a multiscale perspective on many-particle systems"
date:   2016-11-20 15:34:13 +0100
tags: [master, graduation project, crowd dynamics]
---

In the first post on my blog, I return to the graduation project I did during my master: *Large-scale multiscale particle models in inhomogeneous domains: modelling and implementation*. I wrote the thesis at the Eindhoven University of Technology (The Netherlands) and was supervised by Adrian Muntean and Andrei Jalba. You can download the report [here](http://arxiv.org/abs/1609.03732).

To support the analysis and investigate how the model holds up in practice, we developed a simulation framework in Python, called  *Mercurial*. You can find it on GitHub [here](https://github.com/0mar/mercurial). More details about the simulation are found in [this post]({% post_url 2016-12-30-mercurial %}).
<!--more-->
## Introduction

In this thesis, I explore the advantages and drawbacks of a new simulation method for large particle systems. These particles can represent anything from bacteria to fish. We apply the technique to model the [dynamics of human crowds]({% post_url 2018-01-04-crowd-dynamics-1 %}).

A particle system is a physical formulation of how a group of particles moves through a certain space and how these particles interact with each other in the process.
This formulation is usually presented as a set of mathematical rules that decide the motion of each particle, based on the current state of the space and the locations and properties of the other particles.
Commonly, particle systems are based on *local* rules, meaning that each particle is influenced mostly by it's neighbours.
This influence can range from attraction to repulsion or mimicry; it varies on the nature of the particles. Because of this flexibility, it is possible to model so many processes using particle models.

When many particles interact with each other simultaneously, sometimes a certain order can be observed. which seems in contrast to the rules of the system. A good example is a flock of birds, manoeuvring through the skies.
Although each bird positions itself purely based on the position and velocity of its direct neighbours, a flock moves so smoothly it might as well be guided by an invisible hand.
Through purely local interactions, the system obtains macroscopic properties, and can even be viewed as a single body.
The same can be seen when observing when pouring a glass of water. Although the liquid is made up of individual molecules that move and collide erratically, the fluid moves in a unified way.

In this thesis, I explore how we can use this *macroscopic* view to model and simulate complex systems with many particles.
We use the two aforementioned microscopic and macroscopic perspective, and combine them into one model.
Doing so yields two descriptions for the same configuration of particles: a discrete one and a continuum one.
By combining the microscopic formulation with the macroscopic one, we can model different kinds of complex behaviours, and accelerate computer simulations.
As a demonstration, we apply the techniques to simulate the motion of human crowds.

## Dynamics of human crowds

Albeit a bit less organised, pedestrians in a crowd exhibit similar properties as a flock or a river.
The following video illustrates how the individual motion of people crossing a large road assimilates into larger moving bodies.

<iframe width="560" height="315" src="https://www.youtube.com/embed/8QVLkI0EK7g" frameborder="0" allowfullscreen></iframe>

In fact, as soon as people start to move in large numbers, the physical properties of the macroscopic body becomes increasingly similar to a thick fluid. This has been observed by mathematicians and physicists before (i.e. by [L. Henderson][1] and [R. Hughes][2]). By modeling the crowd as both particles and fluids, we have more freedom in mimicking real crowd behavior. However, the combination of these two perspectives into one model poses some mathematical challenges, especially for a complex system like a crowd of people. This makes it an interesting research subject.

<img src="/assets/microscopic_scene.png" alt="Particle representation" style="width: 300px;"/>

In this image you see a simulated environment with pedestrians as black dots. Their goal is to reach the red square, while avoiding the black obstacles.

<img src="/assets/macroscopic_scene.png" alt="Fluid-like representation" style="width: 300px;"/>

This image displays the alternative *fluid-like* representation of the crowd. Instead of the positions of individual particles, we track the density of the crowd in several locations.

This way of modeling has two advantages.

1. Computationally, it becomes hard to simulate crowds with particles when their numbers become higher and higher.
Simulating fluids does not have this drawback. On the other hand, fluid simulations for moving people become imprecise when simulating thin crowds, where particle simulations have no difficulties.

2. The precise nature of interaction between moving people is very difficult to catch in a mathematical model, because of the many factors at play. People in dense crowds have less degrees of freedom, making the combined perspective more accurate.


## Mathematical formulation

In modeling crowd dynamics, two important elements are transport and interaction. We take care of the transport on a microscopic level by using a set of stochastic differential equations generated by a potential field.

The potential field is a function representing the domain and its obstacles. It can be represented like in the image below. The pedestrian choose their path similar to a marble rolling downwards on the graph: the path of steepest descent.

<img src="/assets/transport_potential.png" alt="Example environment" style="width: 300px;"/>

The stochasticity ensures that some randomness occurs in the movement of the pedestrians: they slightly deviate from the path determined by the potential field.

Simultaneously we model interaction on the macroscopic level using a continuity equation modified with a pressure term. The pressure term ensures that in regions of high density the crowd becomes *incompressible*: there is no space for extra pedestrians to fit in.

The continuity equation states that mass (in this case: a crowd) can only be transported by a continuous flow  (walking from point A to B). It represents the core property of a moving crowd.

Pressure is applied to maintain incompressible zones. For a fluid, the meaning of incompressibility is intuitive. For pedestrians, it means that no more people can enter this high-density zone and any approaching pedestrians prefer to  avoid it.

## Microscopic formulation

Let \\(N\\) be the number of individuals in the environment \\( \Omega \subset \mathbb{R}^2 \\). We define particles \\(i =1,\dots,N\\) with positions \\(\mathbf{x}_i(t)\in \Omega\\), velocities \\(\dot{\mathbf{x}}_i(t)\\) and accelerations \\(\ddot{\mathbf{x}}_i(t)\\) at time \\(t>0\\).

Particle motion is defined by the stochastic differential equation

$$\ddot{\mathbf{x}}_i(t) = g\left(\nabla\Phi(\mathbf{x})\right) + \eta(t),$$

where \\(\eta(t)\\) is a noise function and \\(\Phi(\mathbf{x})\\) is a potential field containing information on the environment (like exits and obstacles).

## Macroscopic formulation

We define a density field $$\rho(\mathbf{x},t) \in \mathbb{R}_{+}$$ , a velocity field $$\vec{v}(\mathbf{x},t) \in \mathbb{R}^2$$ and a pressure field $$p(\mathbf{x},t) \in \mathbb{R}_+$$ be the density. For all fields, we keep $$\mathbf{x} \in \Omega$$ and $$t>0$$.

We are able to model congestions, repulsion and flock-like behavior with the partial differential equation

$$\frac{\partial \rho}{\partial t} =- \nabla\cdot {\rho(v-\nabla p)}.$$

This equation represents the continuity of motion, combined with Darcy’s law. Darcy’s law states that the velocity field of a fluid or gas v in a inhomogeneous domain (like a environment filled with obstacles)  is proportional to the pressure gradient \\(\nabla p\\). Thus, in order to correct for the pressure the crowd exerts on its individuals, we correct the velocity field that governs the transport with the pressure gradient.

We obtain the density and velocity by interpolating the particle system. By viewing each particle $$i$$ as a Dirac distribution $$\delta_{\mathbf{x}_i(t)}$$ (centered in $$\mathbf{x}_i(t)$$) we represent the density and velocity field as follows:

$$\rho(\mathbf{x},t) = \sum_{i=0}^N h_{\mathbf{x}_i(t)}(\mathbf{x}),$$

$$v(\mathbf{x},t) = \frac{\sum_{i=0}^N\dot{\mathbf{x}}(t)h_{\mathbf{x}_i(t)}(\mathbf{x})}{\rho(\mathbf{x},t)}.$$

In these expressions, \\(h\\) is a finite-width approximation of the point mass.

Combining these formulations, we are able to avoid the $$\mathcal{O}(N^2)$$ cost of computing particle-particle interactions and still obtain decent interaction between individuals and their environment.

If you’re interested, do take a look at the report itself!

[1]: https://www.researchgate.net/publication/224010877_On_the_fluid_mechanics_of_human_crowd_motion
[2]: http://www.annualreviews.org/doi/abs/10.1146/annurev.fluid.35.101101.161136
