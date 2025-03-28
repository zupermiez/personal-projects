---
title: "Neural network lake detection"
date: 2025-03-02 10:00:00 +0200
categories: [AI, software, data]
tags: [AI, software, data]
image: /assets/img/cnn.jpg  # Add cover image
---

## Process all lakes in Finland to detect certain shapes of personal interest.

I trained a YOLO v8 computer vision model using Pytorch and data (30k images) from the internet. I leveraged OpenStreetMap and Overpass API to get all lakes in Finlands borders exceeding certain area threshold(2,500m^2). Then using OpenCV, I processed the +100k image dataset to make it look like my bitmap training data.Then I use the model to detect and save positive images to a new folder. OSM data doesnt sadly always match with one in f.e. Google Maps and the CNN exhibits "some" overfitting but it has enabled me to do what I set out to do.



![Results](/assets/img/results.png)
*Training graph from one of the previous runs. I halted the training early with the best model so unfortunately i do not have graphs for that*

![Map data](/assets/img/greenmap.jpg)
*Before choosing the OSM API my other option was to scan satellite images using opencv contour detection.*


### Learning
This project interested me because it combines machine learning with very concrete real world data and enabled me to find patterns where no one else has looked and produce some real word results. Training a neural network gave a lot of insight and understanding of how CNNs work. I Also learned of the importance of having high quality data, because the first 16 training runs did not produce sufficient results. I learned about the huge power of opencv with image processing and even data annotating.

### Links
I built this in 6 weeks during the builder program [Sidequest](https://mysidequest.xyz/s1/projects)

### Code
The project code can be found on my [github](https://github.com/zupermiez/thejohnsonproject)


### Results
Below are some personal favorites from the detection folder.


<p float="left">
  <img src="/assets/img/final/lake_60.032732_24.490773.png" width="325" alt="Lake at coordinates 60.032732, 24.490773" />
  <img src="/assets/img/final/lake2.png" width="400" alt="Lake detection result 2" />
</p>
<p float="left">
  <img src="/assets/img/final/lake_61.989647_27.664928.png" width="325" alt="Lake at coordinates 61.989647, 27.664928" />
  <img src="/assets/img/final/lake4.png" width="275" alt="Lake detection result 4" />
</p>
<p float="left">
  <img src="/assets/img/final/lake_62.760774_24.623631.png" width="325" alt="Lake at coordinates 62.760774, 24.623631" />
  <img src="/assets/img/final/lake5.png" width="400" alt="Lake detection result 5" />
</p>
<p float="left">
  <img src="/assets/img/final/lake_62.710499_27.962578.png" width="325" alt="Lake at coordinates 62.710499, 27.962578" />
  <img src="/assets/img/final/lake3.png" width="400" alt="Lake detection result 3" />
</p>
<p float="left">
  <img src="/assets/img/final/lake_63.705917_29.636578.png" width="325" alt="Lake at coordinates 63.705917, 29.636578" />
  <img src="/assets/img/final/lake1.png" width="300" alt="Lake detection result 1" />
</p>

