---
layout: post
title:  "What is crowd dynamics?"
hidden: true
date: 31-12-2017
tags: [mathematics, modelling, crowds dynamics]
---
*This is the first post in a series of entries on the modelling of crowd dynamics.*

In this post I give a short introduction into a topic of my research: crowd dynamics.
Crowd dynamics is a very multidisciplinary field of research; contributions come from a great many fields in social science and natural science.
I will be sticking to my background and focus mainly on mathematical, physical and computer-science aspects.
For the interested reader I have included links to Wikipedia pages of related concepts.

## Definition
Crowd dynamics is the term used for describing the behaviour and interaction of groups of people moving in a certain environment. The main goal of crowd dynamics research is to identify the factors that control these dynamics (analysis), and to be able to predict crowd motion in new scenarios (simulation).

As you can imagine, crowd simulation serves many purposes, like

* Preventing crowd disasters at a large public event.
* Optimizing the layout of a supermarket.
* Designing evacuation strategies for a building.
* Creating realistic animations in a game.

The dynamics of crowds vary from situation to situation. Pedestrians in traffic have different goals and therefore different patterns than people in shopping malls.
Compare across cultures, and you will find even more diversity. This makes the modelling and simulation of crowds a challenge that requires more than just a mathematical understanding of how people move from A to B.
<!--more-->

## Finding order in coherence
However, the fact that patterns vary does not mean that there are not patterns to be found. As sociology tells us, people tend to behave differently in groups than they would alone. The same observation is true in crowds; not only because of psychological effects, but simply because people need to coordinate their movements as to not bump in to each other. Walk through a busy train station, and you will notice there are some aspects of your behaviour which you adapt immediately, without thinking about it.
Because of the larger number of people moving in a constrained space, new structures tend to occur. These structures, called [emergent](https://en.wikipedia.org/wiki/Emergence) effects, can only be observed on a larger scale: circle-shaped congestions in narrow paths, the formation of interlocking lanes.

There are more large-scale observations to be made: if you walk through the same train station every day, you might notice how you never take the exact same path twice. Even if your destination is the same, your path is always altered by small deviations; perhaps when you pass another pedestrian, or when you avoid someone coming the other way.
Still, your small deviations are barely noticeable when you observe the entire crowd. Stated more strongly: the larger the crowd, the more regular it behaves.

These phenomena are not limited to crowds: if you examine a liquid that is made up of molecules, or a body made up of cells, you will find similar properties; characteristics of transport change when you have a large number of 'particles', and
while it is difficult to correctly predict the motion of single entities, the collective acts very orderly.
The field of research that studies with these systems is called [statistical physics](https://en.wikipedia.org/wiki/Statistical_physics).
Statistical physics tells us that even if we are not able to correctly predict the motion of a single individual, we can derive valid conclusions by looking at the collective.
For readers interested in this field, I recommend the informative and accessible book *Critical Mass*, by Philip Ball.

## Finding optimality in order
Actually, crowds often exhibit an even stronger concept of collectiveness; one in which the motion of all its members becomes so streamlined that it can be considered *optimal*. This is another example of emergent behaviour, called [self-organization](https://en.wikipedia.org/wiki/Self-organization).
The only requirement for these type of systems is that every individual (in this case called an *agent*) has control over his own actions and is able to interact with other nearby agents. This is enough to see that on a global scale, the group is much more efficient than pure self-promotion would suggest. In computer science, this structure is called a multi-agent system; artificial intelligence researchers use this phenomenon in building state-of-the-art decision systems mimicking the human brain, otherwise known as [neural networks](https://en.wikipedia.org/wiki/Artificial_neural_network).
On smaller scales, the concept can also be found in biology, for example in flocks and colonies. In [this post]({% post_url 2017-10-20-crowds-and-populations-a-visual-exploration %}), I have a couple of videos showcasing this phenomenon.

I hope I managed to illustrate how broad the field of crowd dynamics can be. The complexity and intersection of so many different disciplines make for a very interesting field of research.

In a next post, I'll describe different modelling techniques used for crowd dynamics, as well as what is to be expected for each of these models.
