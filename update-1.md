title: Literatre Review
layout: default
permalink: /csce645/update-1

## Project Update 1
---
### Summary
The goal for the first few weeks of my project was to setup the initial pipeline required for smoothing the boundaries in segmented images. This involved researching and developing code to carry out the image processing steps and then testing out simple skeleton construction techniques. I used MATLAB and Python to carry out these initial tasks, but I plan on having the entire pipeline working on Python alone.

The first step in the pipeline was to find the boundary pixels for each class of objects present in the image. This is a necessary step as the boundary representations of the individual classes will be used to develop and generate the skeleton of the individual objects. While there are a lot of edge detection techniques that could easily be used to obtain the boundaries within the images, these either work on grayscale images and do not always provide the accurate boundaries, or they do provide the boundaries but not in the right sequential order. For that reason, I initially developed my own code that searched across the grids of pixels and obtained all the boundary pixels on both sides of a given boundary (desired), by searching the top, bottom and right pixels to a given pixel. However, the problem with this approach was that given the boundary pixels of a class in a random order, it was very hard to obtain a counter-clockwise/clockwise ordering of the pixels, specially for shapes that were concave. And though some methods exist that work for ordering coordinates for concave shapes.
