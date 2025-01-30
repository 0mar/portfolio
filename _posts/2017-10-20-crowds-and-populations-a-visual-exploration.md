---
title: "Crowds and populations: a visual exploration"
date:   2017-10-20 20:25:23 +0100
hidden: true
tags: [population dynamics, videos, crowd dynamics]
---

No formulas or commands in this post. Instead, I uploaded a couple of video clips I recorded in various places, showcasing dynamics in different kinds of populations. They are a bit amateurish, but they should be able to demonstrate the complexity present in population dynamics.
Each scenario has a bit of context describing a mathematical model associated with it.

## 1. Crossing pedestrians

The first clip was shot in the Shibuya crossing in Tokyo, Japan. It is known for being one of the most crowded intersections in the world.
Two special features of the crossing: it has more than 4 lanes, and the traffic lights give way to pedestrians from all directions at the same time.

<!--more-->
<iframe width="560" height="315" src="https://www.youtube.com/embed/8QVLkI0EK7g" frameborder="0" allowfullscreen></iframe>

Looking at the video, the pedestrians barely seem to be hindered by the multitude of conflicting directions.
They seem to choose paths that minimise any chance of collision, but without allowing for too much detour. This phenomenon is often called *self-organisation*: a global state of harmony and optimality occurring when each agent fulfils its individual goal while keeping the goals of the collective in mind. This is a subtle process that tends to occur in places where a great many (same-species) non-conflicting organisms gather and interact. Other examples are cells in a human body, transporting energy and water, or vegetation in a jungle, capturing sunlight.

Self organisation is an *emerging* property, meaning that there are no global rules that govern it. In the case of the crossing: there is no predetermined layout of pedestrian paths which resolves all the collisions; it happens while the agents interact with each other locally.

Often, if one is able to find the self-organisation principle in a group of agents, their dynamics become much clearer. Conversely, if you try to make a system that consists of many (virtual) agents, implementing some sort of self-organisation can greatly increase the efficiency of the system.

## 2. Foraging ants

The second clip was recorded in Canadian Museum of Nature, in Ottawa. It features a multitude of ants (consider yourself warned) in a terrarium, processing a food source.

<iframe width="560" height="315" src="https://www.youtube.com/embed/v2DJ35LxyOY" frameborder="0" allowfullscreen></iframe>

Compared to humans and many other flocks of animals, groups of ants have peculiar but fascinating features.
Ants form a colony, which binds them much stronger than other herding animals, even over long distances.
Additionally, they rely mainly on *pheromone trails* left by other members of the colony: when one of the exploring members in a colony finds food, he tries to find his way back to the nest, leaving a chemical (pheromone) that tells other ants to follow the path the exploring ant just came from.
Another unique aspect of ant colonies: they are with very, very many. This makes up for their very limited vision and (individual) brain capacity.

The same technique has been applied in maths, not only to gain insight in the way that ant colonies explore the world, but also to computationally explore mathematical objects that are too complex to analyse theoretically. If it is possible to express the object virtually, then sometimes it is beneficial to release a colony of virtual ants, leaving the exploring and communicating to them.
For more information about pheromone trails and a Python demo, check [this post]({% post_url 2018-02-12-antennae %})

## 3. Aligning school of fish

Next is a school of fish, from the Ripley's Canadian Aquarium in Toronto. The fish were kept in a cylindrical aquarium where the water was vortexed, creating an effect similar to that of a treadmill.

This setup eliminates the forward movement of the school, making it easier observe the group behaviour in individual fish.

<iframe width="560" height="315" src="https://www.youtube.com/embed/l34WP_8SW3E" frameborder="0" allowfullscreen></iframe>

The video shows that the fish school is another example of self-organisation: a stable formation in which each fish maintains its place.
Looking carefully, you can see that when one fish wavers from his position, this deviation creates a wave that propagates through the school.
The fish respond to each other by trying to maintain their relative position in the school.

One of the first models of flocking behaviours in animals is called the boid (bird-oid) model. It was postulated on three principles:

- Avoid collisions your nearest flock members
- Copy the average direction of the school
- Move towards the centre of your free flock space

Real flocks are admittedly more complex than this. Still, try to apply these principles to what you observe in the clip and determine how well the model fits (this instance of) reality.

## 4. Competing school of fish

The last clip displays a group of koi fish in a pond in Hasselt, Belgium. A second clip featuring many fish in a small space. But where the previous fish tried to respect their school, these fish just want to be fed.

<iframe width="560" height="315" src="https://www.youtube.com/embed/cNDg2t9DfYc" frameborder="0" allowfullscreen></iframe>

Trying to obtain the (hypothetical) food, the fish compete for the sweet spot. Naturally, this subverts the principles of self-organisation: each fish puts in a lot of effort, and for the majority this does not pay off. There is no harmony or optimality to be found, except for the right of the strongest.

Still, this is an important category of population dynamics as well: animals competing for resources and trying to maximise their own success. This is a model used for predicting the movement solitary animals in biology. Using differential equations it is possible to model their movement as a function of the resources available.
