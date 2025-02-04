---
permalink: /projects/albatross
---

# AI for nature: protecting the Atlantic Salmon

## Introduction
The Atlantic salmon is one of Norway's most important export products, accounting for 17.5% of the total export trade.
The rivers in Northern Norway are full of them, where they migrate upstream from the ocean to spawn in freshwater during summer and autumn, with young salmon spending 2-3 years in the river before heading to sea.
The last few decades have seen an increase of the artificially introduced pink salmon (also called "humpback salmon"), an invasive species that competes with the Atlantic Salmon for resources and territory.
Unlike the Atlantic variant, the mature pink salmon does not tolerate fresh water, and dies after laying eggs, slowly decomposing upstream in the river. This alters water chemistry and does not only impact the other fish species, but the river ecosystem as a whole.

![fishermen]({{ site.url }}{{ site.baseurl }}/assets/images/fishermen.png){: .align-left} 
_While the problem is widely known, solutions have been mostly manual up to this point. That makes them unsustainable, hard to scale, or ineffective._

Together with a consortium of local partners, we created an AI-powered solution to separate these fish species without causing harm to either of them.

![cage]({{ site.url }}{{ site.baseurl }}/assets/images/cage.png){: .align-left width="50%"} 
_The pink salmon enters the underwater cage, while the other fish species can swim through the door uninterrupted._

## Overview

![fish]({{ site.url }}{{ site.baseurl }}/assets/images/fish.png)

The solution combines machine learning with edge computing and underwater robotics. Here's how it works:
1. An underwater camera streams images to a local edge device.
2. This edge device uses a machine learning model to recognize fish swimming in the images. 
3. Based on the species identified, an automated gate system directs the fish to different paths.
4. Model predictions and image data is uploaded and stored into the cloud, where it is presented in a web interface for river managers

![system]({{ site.url }}{{ site.baseurl }}/assets/images/system.png)
This system runs without human intervention, on solar power, with high accuracy for the relevant species. 

Simula was responsible for the development and deployment of the computer vision model and the software suite. 
I designed the data and software infrastructure, with the goal to run as robustly as possible.

This project was first deployed in Berlevag, Norway in 2022 and has been deployed into more rivers every year since. We are improving the solution to increase accuracy, streamline the hardware and make it more scalable. 

<!-- Todo
1. Add GIFs
2. Add metrics and results on accuracy
3. Perhaps write some blog posts on setup details
4. Share more about infrastructure -->