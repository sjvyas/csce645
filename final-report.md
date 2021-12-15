title: Final Report
layout: default
permalink: /csce645/final-report

## Final Report
---
### Problem Summary

![Image](/assets/images/proposal_pic_00.png) *Semantically segmented image showing the high degree curvature associated with the boundary of the trees. The image is taken from the [RELLIS-3D dataset](https://arxiv.org/abs/2011.12954)*

Image segmentation is a widely used technique to identify regions within images that share similar properties, such as regions belonging to the same object, similar classes of objects, similar color profiles, similar textures, etc. This is particularly useful in applications such as feature recognition, self-driving cars and medical imaging. In applications such as self-driving cars, understanding what the segmented regions consitute and their general shape definitons is very important. Therefore, semantically segmenting images by the class of objects (eg. humans, buildings, trees, etc.) has become a common approach in such applications. Taking the self-driving cars as an example, semantically segmented images can help computers detect the different kinds of obstacles present in the scene, the different types of signs present on the road, traffic signals, etc. Combined with LiDAR point clouds, segmented images can also be used in 3D modeling the objects present in the scene [[2](https://ethz.ch/content/dam/ethz/special-interest/baug/igp/photogrammetry-remote-sensing-dam/documents/pdf/isprs_2012_demir_baltsavias.pdf)]. Semantically segmented images can therefore, provide detailed information (such as the exact shape of the objects) of the scene. However, a lot of objects present in the scene can have boundaries that are represented by very high degree curves, such as boundaries of vegetation (such as trees and bushes), silhouettes of humans, water bodies (such as puddles) and dirt trails in offroad conditions and so on (ADD FIGURE). Images, especially high-resolution images, are a high dimensional data source. They capture the high degree curves of the shapes of objects within the scene with great detail and can therefore lend more complexity to the shapes. While accurate representations of the objects is necessary in certain cases, such as the shapes of obstructive objects for self driving cars (e.g., humans on the road, fences, poles, etc.), it can lead to difficulty in reconstructing the geometric models of these objects. Generating 2D meshes, for example, will lead to much finer and complex meshes near the higher degree curved boundaries of the objects (e.g., small branches of trees), which might not necessarily be of much importance to a car driving at a much lower height of the branch. As such, having a multi-level of detail representation of these objects can be more useful rather than having the same high-level of detail for all the objects within the scene.  

The problem of multi-level of detail representation of the objects can be mitigated in the image space, where complex shapes containing high degrees of curvature, such as the branches of trees can be simplified into simpler, less complex shapes with somewhat flatter boundaries, thus resulting in simpler, less complex shapes. One simple approach to this problem is isolating the individual segmented objects from the scene and reshaping them through a variety of boundary smoothing techniques (for eg., polygonal approximation). However, the big drawback in this method is that refitting individual shapes into the image might cause the following problems: overlaps between shapes and discontinuity between boundaries of shapes within the scene. Therefore, the main goal of this project is to explore ways in which the boundaries of objects within the semantically segmented images can be geometrically simplified into different levels of details, while maintaining continuity in within the scene of the image. 

### Previous Works

![PolygonalApprox](/assets/images/polygonal_approx.PNG) *Examples of the polygonal approximations for real contours generated in [[6](https://www.sciencedirect.com/science/article/pii/S0031320309002532?casa_token=oi8Guh87ZpsAAAAA:ZuHgsIiqNqQUmMKQ9Yuqnvd5N-v4YoNuJWNj5uXBrgqw9KyUUR74ZBe6NYIR_5kq2sj05xgMeew)]*

Boundary smoothing and simplification has been extensively studied both in the fields of computer vision and geometric modeling. Techniques that use geometric methods for smoothing generally focus on simplifying the boundary curves into smoother curves that can still preserve the general shape of the objects or the original curves. An important geometric technique that motivated prior and more recent works is the identification of dominant points present on the boundaries of shapes, i.e. points with high degree of curvature, containing important shape information. One such prior work by Garcia et al., [[1](https://www.sciencedirect.com/science/article/pii/0098300494900469)], achieved boundary simplification, by first segmenting the boundary curves into multiple segments, each of which highlighting specific features of the shape, and then finding the dominant points within these line segments. They finally determine the specific degree of simplification for each segment by using Gaussian smoothing, and minimizing the  normalized measure of zeros of curvature of the segments. Garrido et al., [[2](https://www.sciencedirect.com/science/article/pii/S0031320397001040)] similarly focused on simplification of boundries using a multi-scale dominant point detection method, combined with linear interpolation between the detected points to form simpler boundaries and therefore shapes. 

Another common approach to boundary simplification is the polygonal approximation of the boundary [[24(https://www.sciencedirect.com/science/article/pii/S0167865510001947?casa_token=cfOUhQPyQJ4AAAAA:QlLaLlcq11H2TtgeCq9KEbTJ7snMK8kz1VwbuwueX0t5DKJBjFg6O4IYtPvdOjNgMlzOf3LGKQo)],[25](https://www.sciencedirect.com/science/article/pii/S0031320306003761?casa_token=nV3nhpo6oOUAAAAA:-3kZfJSeivyyTT03bms2jl4-sdyejB780GTvijkTlx55Asj44A3KYTRVK6tF7ZO1XB2spLk6Yw4)]. Prior seminal works include the work done by Ramer [[5](https://www.sciencedirect.com/science/article/pii/S0146664X72800170)], where they presented an itrative procedure to obtain polygonal approximation of 2D curves. The algorithm approximates the 2D curves as polygon with small number of edges. More recent approaches, such as the work by Poyato et al., [[6](https://www.sciencedirect.com/science/article/pii/S0031320309002532?casa_token=oi8Guh87ZpsAAAAA:ZuHgsIiqNqQUmMKQ9Yuqnvd5N-v4YoNuJWNj5uXBrgqw9KyUUR74ZBe6NYIR_5kq2sj05xgMeew)], combines the polygonal approximation approach with that of dominant point detection. In their work, they approximate polygons of planar curves through specialized techniques of finding the dominant points of the curve. Ai et al., [[22](https://www.tandfonline.com/doi/pdf/10.1080/13658816.2016.1197399?needAccess=true)] also proposed a way for simplification of polylines by generating an envelope structure around the polyline using Delaunay triangulation. 

The above geometric-based methods provide a wide range of ways to smooth the boundaries of objects or edges detected in images. However, semantically segmented images provide a richer source of information, i.e., the semantic labels associated with each segment or region in the image, which can be better used by analyzing the shapes associated with the different labels. One such technique that enables this is skeletonization.


![Skeletonization](/assets/images/skel_voronoi.PNG) *Examples of the Hierarchical Voronoi Skeletons generated in [[9](https://www.sciencedirect.com/science/article/pii/003132039400105U)]*

Skeletonization is an effective way of analyzing shapes of objects. Geometrically, skeletonization is the process of transforming a 2D or 3D object into a line representation. The seminal work by Olgniewicz et al., [[9](https://www.sciencedirect.com/science/article/pii/003132039400105U)] introduced hierarchic voronoi skeletons, where they first regularized voronoi diagrams to first obtain basic skeleton structures of the object, and then developed a hierarchic organization of the skeleton constituents. Parameters associated with this process were used to obtain skeletons of different resolutions. However, this technique of cleaning-up the skeleton (pruning), did not guarantee topology preservation. Bai et al., [[10](https://ieeexplore.ieee.org/abstract/document/4069261?casa_token=Ge-1Q7E_7_EAAAAA:h2-Ka0b8-Fw9n7V4N5HqJB054XEaudE32Qvh22SOe_BQ3YHG4oEQ4t7y2Y3uQL_qaYLZBhHroA)] introduced a skeleton pruning method that was based on contour partitioning and could preserve the topology of the original skeleton while preventing spurious branches from being produced. They specifically used Discrete Curve Evolution (DCE) to partition the contour. 

Skeleton pruning is an important step in generating accurate and stable skeletons of shapes and objects. Skeleton pruning can also be used to recreate smoother versions of the original shape, as shown by Tek et al.,[[11](https://link.springer.com/article/10.1023/A:1011229911541)]. They proposed a shape smoothing method based on the analysis of the underlying medial axis of the shape and then iteratively removing the skeletal branches of the shape to obtain a smoother shape. Their method of skeleton pruning focuses on removing the instability of skeleton deformations through symmetry transformations, where the transform is applied to all symmetry branches, which may result in more appropriate smoothing of the shapes. They call this transform the splice transform, which prunes a branch, merges the two segments of the base branch into one and then makes appropriate corrections to the coupled skeletal branches. This method allows them to construct and preserve coarse-scale curvature extrema. My approach to the problem is therefore, motivated by the works that used simplified skeleton representations to reconstruct shapes.

*Note: I have utilized the following python libraries in this project: numpy for array storage and operations, matplotlib to plot images, opencv to read images, scipy.spatial to compute voronoi diagrams*

### Methodology
The methodology I followed in this project was specifically tailored to high-dimensional (high-resolution) semantically segmented images. I took motivation from the past works that used the Medial Axis (MA) not only as a shape descriptor but also to reconstruct simpler shapes through MA regularization. The main steps I followed are given below:
- First, I obtained the boundary pixels of individual objects/shapes within the segmented images. The boundary pixels were transformed to x-y coordinates, thus, allowing me to work in the object space from the image space.
- In order to obtain the skeletal representation of the objects, and eventually the Medial Axis, I used Voronoi Diagrams, which are known to produce good approximations of the Medial Axis, using the boundary coordinates of the objects as Voronoi sites.
- Due to a high number of boundary points, the skeletal representation of each object was dense with branches as it captured all the boundary noise. In order to get rid of the noise induced brances, and obtain a fairly simple Medial Axis, I implemented a skeleton pruning technique.
- The pruning was done on a rule-based approach, depending on the size and/or class of the object. This accounted for the Multi-Level of Detail Representation of objects within the scene.
- Finally, to reconstruct the shapes (objects) from the pruned skeleton of the objects, I used the skeleton points as Voronoi sites and constructed the Voronoi Diagram respecting the semantic labeling of the individual objects.

I will now explain the above steps in detail.

***Boundary Extraction***


![fig010](/assets/images/missing_contour.png) *Figure. Missing boundary pixels using the OpenCV find_contours function.*


![fig001](/assets/images/boundary_extraction1.png) *Figure Boundary extraction using Moore's Neighborhood approach*

Boundaries are essential to describe the shape of 2D objects. While a number of techniques exist for boundary extraction from images, the challenge in my case was to accurately obtain individual boundaries of each object such that the extracted boundary could accurately represent the shape of the objects. While quite a few boundary extraction functions exist in popular python libraries, such as [OpenCV](https://docs.opencv.org/3.4/d4/d73/tutorial_py_contours_begin.html) and [scikit-image](https://scikit-image.org/docs/dev/auto_examples/edges/plot_contours.html), they returned incomplete contours/boundaries. As a result, I implemented the Moore's neighborhood tracing algorithm. Specifically I tweaked the algorithm borrowed from [Wikipedia](https://en.wikipedia.org/wiki/Moore_neighborhood) to extract boundaries of all objects along with their class labels in one go. An example of the output of the boundary extraction algorithm is show above. The extracted boundaries were also represented as x-y coordinates for ease of work at later stages of the project.

***Skeletonization***
![fig01](/assets/images/voronoi01.png) *Figure. Skeletonization using the Boundary points as Voronoi Sites*
Skeletal representations are an important way of representing the shape interiors. Along with the boundary, they can be used to describe the interior and exterior of the shapes. Therefore, to simplify an object's shape, computing its skeleton becomes an important step in my process. 

I used the Voronoi-based approach to generate the skeletons of the objects. A 2D Voronoi diagram partitions a plane into regions called Voronoi cells. When constructed using the boundary points of a shape as the Voronoi sites, the edges of the cells lying within the shape can be closely approximated to the skeleton of the shape. I used the boundary points of all the objects within the image as voronoi sites and constructed the voronoi diagram. This gave me an extremely dense skeleton structure for each object due to the large number of boundary points corresponding to their shape (FIGURE).

***Skeleton Pruning to Obtain Approximate Medial Axis***
![fig01](/assets/images/movie.gif) *Figure. Skeleton Pruning on the Puddle object*
The next step was to prune the skeleton structures of the individual objects in order to simplify the skeleton and obtain the Medial Axis of the different shapes. This was one of the more important steps of the process since the concept of Multi-Level of Detail Representation could be explored as a way of simplifying the skeleton structure of different classes of objects to different degrees. 

These were the steps I followed to prune the skeleton:
- Obtain skeleton from individual objects within image.
- Identify the valency of the skeleton points i.e., find the number of edges attached to a given vertex. This step also helps me identify the edges that are connected to these edges.
- Iteratively remove edges containing one valency skeleton points, that are the leaf nodes, i.e., vertices connected to only one edge, which occurs at the outermost branches of the skeletal representation.
- Remove till threshold is reached.

The thresholding strategy is important as it decides how complex or simple the pruned skeleton is going to look. For my thresholding strategy, I use the number of skeleton edges present within the object as an indicator as to whether the threshold should be kept high or low. Keeping a standard threshold for all the classes may cause either the larger objects to have more spurious branches, or smaller objects to have a disappearing medial axis. Therefore, having different thresholds for objects with different number of skeleton edges is beneficial. 

I calculate the threshold by dividing the number of edges removed in one iteration of pruning to the original number of skeleton edges present in the object. This method finds out when the pruning is reducing signifying that the pruned skeleton is nearing the approximate medial axis.

**Multi-Level of Detail**
In order to have a multi-level of detail approach to this problem, I allow the preservation of shapes of objects belonging to specific classes. This is done by adding the original skeleton of preserved class as well as the skeleton edges surrounding the boundary of the preserved class that lend to the high level of detail of the objects. As a result, I can have certain classes of shapes that have a high-level of detail and the rest of the classes have a medium-to-low level of detail representation. 

***Shape Reconstruction and Resegmentation***

Once the pruned skeleton points and their corresponding class labels of the objects were obtained from the previous step, the last step of the process was to reconstruct the simplified object shapes. Using the skeleton points as voronoi sites, I constructed a Voronoi diagram, where each cell was colored using the sites' class labels. The advantage of using Voronoi diagrams to reconstruct the shapes is its' space-filling property. As a result there were no void gaps between objects, and for the most part, the boundary relations between the objects were similar to the original image, thus the image was, in some sense resegmented.

***Challenges and Failures***

Apart from the nature of the results, the major challenges I faced was in developing an algorithm to compute the Voronoi diagrams. My approach was to use the [Bowyer-Watson](https://en.wikipedia.org/wiki/Bowyer%E2%80%93Watson_algorithm) algorithm to compute the Delaunay Triangulation, and then compute the Voronoi diagrams which is its dual graph. Unfortunately I was running into many issues caused by degenerate cases and due to the high number of data points that I was using. As a result, I resorted to using the [function in the scipy library](https://docs.scipy.org/doc/scipy/reference/generated/scipy.spatial.Voronoi.html) to compute the Voronoi diagrams. 

The other challenges I faced was to set an appropriate threshold at which to stop the skeleton pruning process for the different objects. As every object had a widely different boundary points, keeping a single low threshold resulted in the disappearance of features for most small objects. Whereas keeping a threshold to allow for more number of skeleton edges resulted in the larger objects having spurious branches. I settled for a size-based approach where the threshold was set depending on the size of the boundary points.

### Results

![FR01_00](/assets/images/final_result_1.png)

### Analysis
The proposed approach is 

### Conclusion and Future Work
To conclude, my proposed approach was able to perform well for certain types of images that contained sharp protrusions. I was able to provide a way to preserve the shapes of classes of objects, however, the current implementation may result in a more perturbed boundary for the preserved class. One potential direction of expanding this work is to find a way of keeping the boundary of the preserved class intact while also respect the skeletons of the neighboring objects. While adding the boundary is simple, re-computing the specific skeleton regions is a difficult task. As mentioned above, the current approach also doesn't perform well when there are multiple holes in a class of objects. This needs to be researched more.

[Link to Home Page](https://sjvyas.github.io/csce645/)
