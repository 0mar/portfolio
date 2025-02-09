---
permalink: /projects/albatross
---

# AI for nature: protecting the Atlantic Salmon

## Introduction
The Atlantic salmon is one of Norway's most important export products, accounting for 17.5% of the total export trade.
The rivers in Northern Norway are full of them, where they migrate upstream from the ocean to spawn in freshwater during summer and autumn, with young salmon spending 2-3 years <> in the river before heading to sea.
The last few decades have seen an increase of the artificially introduced pink salmon (also called "humpback salmon"), an invasive species that competes with the Atlantic Salmon for resources and territory. <> (add newspaper)
Unlike the Atlantic variant, the mature pink salmon does not tolerate fresh water, and dies after laying eggs, slowly decomposing upstream in the river. This alters water chemistry and does not only impact the other fish species, but the river ecosystem as a whole.

![fishermen]({{ site.url }}{{ site.baseurl }}/assets/images/fishermen.png){: .align-left} 
_While the problem is widely spread, solutions have been largely manual or mechanical up to this point. That makes them unsustainable, hard to scale, or ineffective._

Together with a consortium of local partners, spearheaded by the Berlevåg fishing and hunting association, we built an AI-powered solution to separate these fish species without causing harm to either of them.

![cage]({{ site.url }}{{ site.baseurl }}/assets/images/cage.png){: .align-left width="50%"} 
_The pink salmon enters the underwater cage, while the other fish species can swim through the door without interruption._

## Overview

![fish]({{ site.url }}{{ site.baseurl }}/assets/images/fish.gif)
_Following a pink salmon that enters from the left, and which will be directed towards the underwater cage._

The solution combines machine learning with edge computing and underwater robotics. Here's how it works:
1. An underwater camera streams images to a local edge device.
2. This edge device uses a machine learning model trained on aquatic wildlife data to identify any species in the picture in real-time.
3. Based on the species identified, an automated gate system directs the fish to different paths.
4. Model predictions and image data is uploaded and stored into the cloud, where it is presented in a web interface for river managers.

![system]({{ site.url }}{{ site.baseurl }}/assets/images/system.png)
_The journey of the fish. No human intervention is necessary for the operation of this system._

Simula was responsible for the development and deployment of the computer vision model and the software suite. 
I led our work package and designed the data and software infrastructure. The goal was to ensure that in spite of the varying and unpredictable real-life conditions, the system would be as robust as possible.

## Infrastructure
- Modules
- Data flow
- Redundancy

## Results
This project was first deployed in Berlevåg, Norway in summer 2022 and has been deployed into more rivers every summer since. We are improving the solution to increase accuracy, streamline the hardware and make it more scalable.

In the last iteration, the summer of 2024, we caught over 5000 pink salmon. The recall rate was over 99%. 

This project won the jury price on the World AI festival in Cannes, France in 2022. It was a top 10 finalist for a United Nations Sustainable Development Goals award in 2023 and won a Mobile Technology in Barcelona, Spain in 2024. <>