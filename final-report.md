title: Final Report
layout: default
permalink: /csce645/final-report

## Final Report
---
### Problem Summary
Image segmentation is a widely used technique to identify regions within images that share similar properties, such as regions belonging to the same object, similar classes of objects, similar color profiles, similar textures, etc. This is particularly useful in applications such as feature recognition, self-driving cars and medical imaging. In applications such as self-driving cars, understanding what the segmented regions consitute and their general shape definitons is very important. Therefore, semantically segmenting images by the class of objects (eg. humans, buildings, trees, etc.) has become a common approach in such applications. Taking the self-driving cars as an example, semantically segmented images can help computers detect the different kinds of obstacles present in the scene, the different types of signs present on the road, traffic signals, etc. Combined with LiDAR point clouds, segmented images can also be used in 3D modeling the objects present in the scene [[2](https://ethz.ch/content/dam/ethz/special-interest/baug/igp/photogrammetry-remote-sensing-dam/documents/pdf/isprs_2012_demir_baltsavias.pdf)]. Semantically segmented images can therefore, provide detailed information (such as the exact shape of the objects) of the scene. However, a lot of objects present in the scene can have boundaries that are represented by very high degree curves, such as boundaries of vegetation (such as trees and bushes), silhouettes of humans, water bodies (such as puddles) and dirt trails in offroad conditions and so on (ADD FIGURE). Images, especially high-resolution images, are a high dimensional data source. They capture the high degree curves of the shapes of objects within the scene with great detail and can therefore lend more complexity to the shapes. While accurate representations of the objects is necessary in certain cases, such as the shapes of obstructive objects for self driving cars (e.g., humans on the road, fences, poles, etc.), it can lead to difficulty in reconstructing the geometric models of these objects. Generating 2D meshes, for example, will lead to much finer and complex meshes near the higher degree curved boundaries of the objects (e.g., small branches of trees), which might not necessarily be of much importance to a car driving at a much lower height of the branch. As such, having a multi-level of detail representation of these objects can be more useful rather than having the same high-level of detail for all the objects within the scene. 

The problem of multi-level of detail representation of the objects can be mitigated in the image space, where complex shapes containing high degrees of curvature, such as the branches of trees can be simplified into simpler, less complex shapes with somewhat flatter boundaries, thus resulting in simpler, less complex shapes. One simple approach to this problem is isolating the individual segmented objects from the scene and reshaping them through a variety of boundary smoothing techniques (for eg., polygonal approximation) (ADD IMAGE). However, the big drawback in this method is that refitting individual shapes into the image might cause the following problems: overlaps between shapes and discontinuity between boundaries of shapes within the scene. Therefore, the main goal of this project is to explore ways in which the boundaries of objects within the semantically segmented images can be geometrically simplified into different levels of details, while maintaining continuity in within the scene of the image. 

### Previous Works

### Methodology
The methodology I followed in this project was specifically tailored to high-dimensional (high-resolution) semantically segmented images. I took motivation from the past works that used the Medial Axis (MA) not only as a shape descriptor but also to reconstruct simpler shapes through MA regularization. The main steps I followed are given below:
- First, I obtained the boundary pixels of individual objects/shapes within the segmented images. The boundary pixels were transformed to x-y coordinates, thus, allowing me to work in the object space from the image space.
- In order to obtain the skeletal representation of the objects, and eventually the Medial Axis, I used Voronoi Diagrams, which are known to produce good approximations of the Medial Axis, using the boundary coordinates of the objects as Voronoi sites.
- Due to a high number of boundary points, the skeletal representation of each object was dense with branches as it captured all the boundary noise. In order to get rid of the noise induced brances, and obtain a fairly simple Medial Axis, I implemented a skeleton pruning technique.
- The pruning was done on a rule-based approach, depending on the size and/or class of the object.
- Finally, to reconstruct the shapes (objects) from the pruned skeleton of the objects, I used the skeleton points as Voronoi sites and constructed the Voronoi Diagram respecting the semantic labeling of the individual objects.

I will now explain the above steps in detail.

***Boundary Extraction***

Boundaries are essential to describe the shape of 2D objects. While a number of techniques exist for boundary extraction from images, the challenge in my case was to accurately obtain individual boundaries of each object such that the extracted boundary could accurately represent the shape of the objects. While quite a few boundary extraction functions exist in popular python libraries, such as [OpenCV](https://docs.opencv.org/3.4/d4/d73/tutorial_py_contours_begin.html) and [scikit-image](https://scikit-image.org/docs/dev/auto_examples/edges/plot_contours.html), they returned incomplete contours/boundaries (FIGURE 1). As a result, I implemented the Moore's neighborhood tracing algorithm. Specifically I tweaked the algorithm borrowed from [Wikipedia](https://en.wikipedia.org/wiki/Moore_neighborhood) to extract boundaries of all objects along with their class labels in one go. YOU CAN FIND MY ALGORITHM HERE. An example of the output of the boundary extraction algorithm is show in Figure 2. The extracted boundaries were also represented as x-y coordinates for ease of work at later stages of the project.

***Skeletonization***

Skeletal representations are important shape descriptors.



[Link to Home Page](https://sjvyas.github.io/csce645/)
