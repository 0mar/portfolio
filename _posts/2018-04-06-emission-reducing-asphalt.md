---
title:  "Modelling the effectivity of pollution-reducing asphalt"
hidden: true
tags: [mathematics, modelling, fem]
---

This is a showcase of a recent project I worked on with colleagues from our mathematics department. In addition to describing the mathematical techniques we used, I want to demonstrate how mathematics can be used to gain insight in real life situations.

I have tried to make this post as accessible as possible. If you are interested in the details; a preprint is available [here](https://arxiv.org/abs/1810.10451), it will be published in *Acta Polytechnica* in early 2019.

The municipality of a small German city approached us regarding the following issue.
Nitric oxides are air polluting chemicals emitted by motor vehicles and are, among others, harmful to the ozone layer.
In a framework of measures to reduce the air pollution, the municipality had decided to install *photovoltaic asphalt*, asphalt coated with a special substance that actively converts nitric oxides like $\textrm{NO}$ and $\textrm{NO}_2$ to nitrate ($\textrm{NO}_3^-$), which clings to the surface of the asphalt until the rain washes it away. Because these nitrates are not airborne, air pollution is reduced.

Due to extensive measurements, there was a lot of information available on $\textrm{NO}$ concentration prior to, as well as after the installation of the new asphalt.

Their question was if we could estimate the influence of the different environmental measures in this particular scenario, to help them make future decisions based on a validated approach.
To answer this question, we created a mathematical model that incorporated the major elements that play a role in this process, so that different scenarios could be explored and emission reducing measures could be compared.
<!--more-->

## Model

The emission of $\textrm{NO}$ from a motor vehicle in an open environment can be modelled with an [advection-reaction-diffusion equation](https://en.wikipedia.org/wiki/Convection%E2%80%93diffusion_equation). This is a partial differential equation (PDE) that is able to approximate the evolution of a gaseous substance subject to diffusion (natural spreading), advection (directional movement due to e.g. wind or rain) and creation/depletion (emission by motor vehicles and degradation to $\textrm{NO}_2$). This gaseuous substance is the nitric oxide and and will be represented with $u$.

This PDE has the following form.

$$ \frac{\partial u(x,t)}{\partial t} = \Delta u(x,t) + f(x,t)$$

where $x$ is a space variable, and $t$ the time variable.
Term by term, this equation has the following meaning: Everywhere inside our environment, the *rate of change* of substance $u$ (denoted with $\frac{\partial u(x,t)}{\partial t}$) is affected by the *diffusion* of $u$ (denoted with $\nabla u$) and the *creation/depletion* of $u$ (denoted with $f(x,t)$).

We account for the geometry of our model (the environment in which the process takes place) by setting specific *boundary conditions* to this PDE: conditions that specify the behaviour of $u$ at the boundary of our environment.
We take a cross-section of the road in question, a schematic drawing of which is included below.

<img src="/assets/pollution_files/geometry.png" alt="The geometry the PDE is solved in" />

Since the ground is solid asphalt or pavement, we assume it cannot be penetrated by the pollutant. We model this with a *Neumann* boundary condition, which states that the outflow of the substance through this boundary must be 0:

$$ - D\nabla u = 0 $$

The photovoltaic asphalt converts the substance $\textrm{NO}$ that comes in contact with its surface to $\textrm{NO}_2$, with a certain reaction rate $\kappa$.
We mimic this with a *Robin* boundary condition, a condition that states the outflow of our substance through this boundary is equivalent to the difference between the concentration of the substance in our geometry, and the atmospheric level. Simply put, high concentrations emit a lot of pollution, while low concentrations emit less.

$$ - D\nabla u = \kappa(u-u_0) $$

The contact with the environment, and consequently, the spreading of the pollutant out into the atmosphere is modelled with a Robin boundary as well. The principle that held for the asphalt also holds here, but for a different reason: The asphalt converts the pollution into natural substances, and the rate of this conversion is proportional to the pollutant concentration.

The model that we created does not account for all effects present.
In order to keep it mathematically tractable, it is necessary to make certain *assumptions* and omit some of the less significant details of the process. While this introduces *modelling errors*, including the most relevant effects will in general still yield useful simulations.

From there, you can refine the model as you obtain more information about the process. As soon as the level of accuracy satisfies your needs, you can adjust the parameters to explore new situations.

## Results

We implemented the model in a computer simulation (using finite element library [FEniCS](https://fenicsproject.org/)).
By using vehicle counting data (freely made available [here](https://www.bast.de/)), we were able to accurately estimate the emitted pollution.

Our first results looked like this:

<img src="/assets/pollution_files/pre_sun.png" alt="Comparison between the simulation and the measured data" />

As you can see, there is a large discrepancy between predicted and measured pollution in the late afternoon. Wondering where we made a mistake in our simulation, we went back to investigate the reaction. One of the initial effects we omitted was sunlight, assuming it was only a minor influence.

As you can see, that assumption was wrong. By adapting the model to take the intensity of the sun into account (with a relation based on [this](https://www.timeanddate.com/sun/) data), we recalibrated the model and got the following new result.

<img src="/assets/pollution_files/post_sun.png" alt="Comparison between the simulation and the measured data" />

The simulations match the measurements nicely. There are still some discrepancies present: the delays of the peaks, the platform between the peaks does not match, but size, as well as shape are nicely recovered. Now, this model can be used to discover the effects of additional/different rules. For instance, decreasing the emission allowed per car, restricting the number of cars, increasing the efficiency of the asphalt.

## Conclusion

We constructed and calibrated a model to simulate the emission and reactions of air pollution in an urban environment. We were able obtain accurate simulations of the process, as well as obtaining a effect magnitude estimate of each phenomenon involved.

To all of you who maintain open sources of data and knowledge, thanks!
Not only the knowledge of the process, but especially the abundance of detailed measurements and data sources available online were indispensable for this project.
