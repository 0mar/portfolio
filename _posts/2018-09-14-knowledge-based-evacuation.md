---
title:  "Knowledge-based crowd dynamics in evolving evacuation scenarios"
hidden: true
date: 2018-09-14
tags: [phd, paper, crowd dynamics]
---

Recently, we published a new article on the evacuation dynamics of human crowds in building environments: [Effects of environment knowledge in evacuation scenarios involving fire and smoke: a multiscale modelling and simulation approach](https://link.springer.com/article/10.1007/s10694-018-0743-x), in the journal Fire Technology.
In this article, we investigate what happens if we mix two kinds of individuals: people who know their surroundings and the fastest way to the exit (called *residents*), and people who are unfamiliar with the environment and who will follow others in their attempt to evacuate (called *visitors*). The goal of this investigation is to gain insight in how knowledge of environment impacts evacuation dynamics.
<!--more-->

## Context
From the second half of the last century, research into evacuation scenarios increased substantially. 
Apart from enacting evacuations in buildings using fire drills, researchers looked at evacuations due to real emergencies, interviewing the occupants after the fact. One example of these investigations is \[[Horiuchi et al.](http://www.iafss.org/publications/fss/1/523)\], where the authors found that evacuation progress strongly varied per floor of the building. Analysis showed that this difference was caused largely due to the fact that some floors were occupied by people working there every day, and others were occupied by attendants to a conference, most of which never had set foot in that building before.

Inspired by situations like these, we proposed a model in which both of these groups are present in a building that needs to be evacuated due to a fire emergency.
Somewhere in the geometry there is a source of fire, creating heat and smoke which impedes the evacuation.

![png](/assets/evacuations/global_view.png)

## Simulation structure

At the beginning of the simulation, the crowd is spread throughout the environment, at a safe distance from the fire. Every occupant has the goal to reach the exit as fast as possible. Since the residents are familiar with their environment, they know how to orient themselves and take the fastest path to the exit. The visitors, however, will follow the people in their vicinity, no matter the direction.

While the residents at first do not know where the fire is located, they will realize that they are moving closer to the fire and find another exit if they sense the increase in heat and smoke.

We implemented these behaviours and environmental conditions in [Mercurial](https://github.com/0mar/mercurial), allowing us to simulate the model for different geometries. If you have difficulties getting the current version to work, drop me a line!

## Observations

By running the simulation for various configurations, we obtained a sense of how the model performs and how the difference in populations affects the evacation. As expected, by changing the ratio of residents to visitors, the structure of the evacuation changes drastically. 

This video demonstrates the model for a large number of residents:

<iframe width="560" height="315" src="https://www.youtube.com/embed/o5dKnTY9whY" frameborder="0" allowfullscreen></iframe>

This video demonstrates model for the same geometry, this time for a large number of visitors:

<iframe width="560" height="315" src="https://www.youtube.com/embed/k1Kj4VekveM" frameborder="0" allowfullscreen></iframe>

You can see that the structure of the crowds is essentially different. Where in the resident-dominated case we see a small number of concentrated groups with a single direction, in the visitor-dominated case we see a large number of smaller groups, dynamically changing focus based on the geometry and their interactions.

The difference in structures translates itself to other aspects as well, like the pressure that the crowd exerts on the individuals, and the time it takes the crowd to reach the exit. These measures are displayed in the figures below.
![png](/assets/evacuations/pressure_plot_residents.png)
This image displays the _crowd pressure_ observed throughout the simulation. Crowd pressure can be seen as a measure of how much the crowd (as a collective) steers an individual into a different direction as the one he intends.

In the case of a resident-dominated evacuation, we can observe that the pressures are higher, especially close to the fire source. The conflicting directions here are due to a lack of information spreading among the residents; these individuals are of the opinion that their route is optimal until they see the calamity for themselves. This causes trouble for the individuals near the fire, since the pressure pushes them into dangerous areas.

![png](/assets/evacuations/pressure_plot_visitors.png)

In the case of a visitor-dominated evacuation, we observe that the pressures are an order of magnitude lower. This is caused mainly due to the flexibility the visitors have with respect to their direction: any sharp changes in directions caused by them approaching a wall or meeting a resident are swiftly assumed by the entire group. While this reduces the risk of being pushed into dangerous zones, it causes a large part of the visitors to spend a lot more time in the geometry, with a lot of sub-optimal paths towards the exit.

![png](/assets/evacuations/evac_times.png)

This figure illustrates the effect of the resident/visitor ratio on the evacuation time. We can see that in spite of what might seem intuitive, there is no proportional relation between the two. On the contrary, as long as the ratio of visitors remains sufficiently low (in this case, up to 35%), the time it takes to evacuate the geometry does not significantly increase, even if the number of visitors does. Apparently, for these configurations, the optimal route information provided for the residents spreads fast enough.

However, when the ratio of visitors increases, evacuations prove to be more and more difficult. After a certain time, all residents have left the building. Without anyone knowledgeable to follow to the exit, the visitors wander around, basing their route only on the shape of the geometry.

## Conclusions

It is essential to model the different internal state of people when looking at evacuations.
In this case, we have illustrated that the difference in knowledge between individuals can have a huge impact not only on how long it takes for a building to be evacuated, but also how the evacuation progresses.
The interaction between people with diffent levels of knowledge plays an important role. Due to leader-follower behaviour, people who would otherwise be lost can safely reach the exit. However, this simulation shows that if the balance between potential leaders and followers is lost, several issues can impede the evacuation.

This is a work in progress. Future plans involve investigating more evacuation scenarios and evaluating how people obtain, retain and act on knowledge. In doing so, we hope to improve the quality of the simulation, and the validity of its conclusions.
One potential improvent is to allow for sharing knowledge between residents 
We hope results like these are useful for practitioners in both evacuation and building development, both as an inspiration and as a tool that can improve the safety of people.
