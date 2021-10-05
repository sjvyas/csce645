<!-- layout: default
title: "Project Proposal"
permalink: /csce645/proposal/ -->
# Simplification of Semantically Segmented Images for Multi-Level of Detail Representation
---
## by Shantanu Vyas
---
![Image](/assets/images/proposal_pic_00.png) *Semantically segmented image showing the high degree curvature associated with the boundary of the trees. The image is taken from the [RELLIS-3D dataset](https://arxiv.org/abs/2011.12954)*

Image segmentation is the process of identifying regions within the image that share similar properties, such as regions belonging to the same object, similar classes of objects, similar color profiles, similar textures, etc. Segmenting images, can therefore, make the analysis of images easier [[1](https://www.sciencedirect.com/science/article/pii/S0734189X85901537)]. This is particularly useful in applications such as feature recognition, self-driving cars and medical imaging. In applications such as self-driving cars, understanding what the segmented regions consitute is very important. Therefore, semantically segmenting images by the class of objects (eg. humans, buildings, trees, etc.) has become a common approach in such applications. Taking the self-driving cars as an example, semantically segmented images can help the computers detect the different kinds of obstacles present in the scene, the different types of signs present on the road, traffic signals, etc. Combined with LiDAR point clouds, segmented images can also be used in 3D modeling the objects present in the scene [[2](https://ethz.ch/content/dam/ethz/special-interest/baug/igp/photogrammetry-remote-sensing-dam/documents/pdf/isprs_2012_demir_baltsavias.pdf)]. Semantically segmented images can provide detailed information (such as the exact shape of the objects) of the scene and LiDAR points, which are inherently more noisy and less detailed, can provide the depth required to better understand the 3D orientation of the objects in the scene. However, a lot of objects present in the semantically segmented images can have boundaries that are represented by very high degree curves, such as boundaries of vegetation (such as trees and bushes), silhouettes of humans, water bodies (such as puddles) and dirt trails in offroad conditions and so on (see figure above). While this provides detailed and accurate representations of the objects, it can lead to difficulty in reconstructing the geometric models of these objects. Generating 2D meshes, for example, will lead to much finer meshes near the higher degree curved boundaries of the objects (e.g., small branches of trees), which might not necessarily be of much importance. As such, having a multi-level of detail representation of these objects can be more useful rather than having the same level of detail for all object classes. 

The problem of multi-level of detail representation of the objects can be mitigated in the image space, specifically in the semantically segmented images, where boundaries of complex shapes containing high degrees of curvature, such as the branches of trees can be simplified into smoother curves, thus resulting in simpler, less complex shapes. This can also be an important technique to represent objects that are 'far' in the scene using fairly simpler shapes by geometrically smoothing the boundaries while maintaining continuity in the labels of the images (i.e., preserving the semantic segmentation of the image). Therefore, the main goal of this project is to explore different ways in which the boundaries of objects within the semantically segmented images can be geometrically simplified into different levels of details, thus, making the image analysis tasks comparatively easier. 

More specifically, my current proposed approach to this problem is to combine the techniques of skeletonization of the 2D objects present in the segmented images using the concept of voronoi skeletons, and then simplifying the skeletons using different skeleton pruning techniques. Skeletonization offers a unique way of analyzing shapes associated with the different classes of objects present in the semantically segmented images, which may allow for class-specific smoothing as well. The idea behind simplification of the skeleton into stable forms through pruning, is that it will enable the reconstruction of smoother shapes from the simplified skeletons. The reconstruction of the shape can be achieved through various ways, such as generating voronoi diagrams, using the pruned skeleton points as the voronoi cites. Other methods will also be explored depending on the favorable representation of the boundaries (e.g., rectangulation form of the boundaries can be beneficial for meshing purposes). The multi-level of detail aspect of the problem can be observed in a variety of different ways: resampling reduced number of points along the skeletal representation of the images, hierarchical approach to pruning of the skeletons, etc. Prior works that have taken a similar approach are discussed in detail in the literature review. More specifically the work by Tek et al.,[[3](https://link.springer.com/article/10.1023/A:1011229911541)] on shape smoothing using skeletonization will form a strong inspiration for this work. However, most prior works focus on single objects or objects present in binary segmented images. The aim of this work will be to create an implementation for smoothing/simplifying shapes across all the classes present in the semantically segmented images, thus providing a differentiating factor from prior works. In addition, preliminary experiments will be conducted on generating 2D meshes of the objects present in the images. This will allow us to see the differences in the complexity of generated meshes for the multi-level of detail representations of the shapes. The mesh generation part of the problem is simply an application and the main focus of the problem is the simplification of the semantically segmented images.

### List of Goals
For this project I will be using the semantically segmented images provided in the RELLIS-3D Dataset [[4](https://arxiv.org/abs/2011.12954)], which contain images of off-road environments.

Primary Goals:
- Generate skeleton representations of the objects present in the semantically segmented images, using voronoi skeleton techniques.
- Explore different skeleton pruning techniques, with focus on the hierarchical approach of pruning to generate multi-level of detail representations.
- Reconstruct segmented shapes using the pruned skeletons, while preserving topology of the objects.
- Explore different reconstruction techniques for representing boundaries in different forms.
- Implement this approach on a multi-class level (across multiple objects in the images) instead of a single class approach.

Secondary Goals:
- 2D mesh generation of the multi-level of detail shape representations.
- Comaprison between the complexity of the generated mesh across different levels of detail.

NOTE: My GAR work focuses on developing 3D geometric scenes of natural environments using semantically segmented images and LiDAR point clouds for autonomous vehicles. The work that will be done in this project will only be a preliminary exploration of the effect of boundary smoothing on reducing the complexity of shapes. The methods used in this project might not necessarily be used for the GAR work due to computation complexity, but may inspire us to explore faster techniques of simplifying boundaries.

### References
[1] Haralick, Robert M., and Linda G. Shapiro. "Image segmentation techniques." _Computer vision, graphics, and image processing_ 29.1 (1985): 100-132.

[2] Demir, N., and E. Baltsavias. "Automated modeling of 3D building roofs using image and LiDAR data." _Proceedings of the XXII Congress of the International Society for Photogrammetry, Remote Sensing, Melbourne, Australia_. Vol. 25. 2012.

[3] Tek, Hüseyin, and Benjamin B. Kimia. "Boundary smoothing via symmetry transforms." _Journal of Mathematical Imaging and Vision_ 14.3 (2001): 211-223.

[4] Jiang, Peng, et al. "Rellis-3d dataset: Data, benchmarks and analysis." _arXiv preprint arXiv:2011.12954_ (2020).

[Link to Literature Review](https://sjvyas.github.io/csce645/literature-review)
[Link to Home Page](https://sjvyas.github.io/csce645/)

