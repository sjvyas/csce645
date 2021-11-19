title: Update 2

layout: default

permalink: /csce645/update-2

## Project Update 2

My main goals for this update were as follows:
- Implement a boundary tracing algorithm that works similarly to the boundary tracing algorithm in [MATLAB](https://www.mathworks.com/help/images/ref/bwboundaries.html)
- Explore skeleton pruning technique to improve the skeletal representation of the different objects present in the image.
- Preliminary exploration of reconstructing segmented images using the simplified skeleton. 
 
My first step was to implement the boundary tracing algorithm. While quite a few boundary extraction functions exist in popular python libraries, such as [OpenCV](https://docs.opencv.org/3.4/d4/d73/tutorial_py_contours_begin.html) and [scikit-image](https://scikit-image.org/docs/dev/auto_examples/edges/plot_contours.html), they returned incomplete contours/boundaries (Figure 1). As such I implemented the Moore's neighborhood tracing algorithm, similar to the MATLAB function. Specifically I followed the following algorithm borrowed from [Wikipedia](https://en.wikipedia.org/wiki/Moore_neighborhood): 

![fig01](/assets/images/missing_contour.png) *Figure 1. Missing boundary pixels using the OpenCV find_contours function.*


    Input: A square tessellation, T, containing a connected component P of black cells.
    Output: A sequence B (b1, b2, ..., bk) of boundary pixels i.e. the contour.
    Define M(a) to be the Moore neighborhood of pixel a.
    Let p denote the current boundary pixel.
    Let c denote the current pixel under consideration i.e. c is in M(p).
    Let b denote the backtrack of c (i.e. neighbor pixel of p that was previously tested)

    Begin
      Set B to be empty.
      From bottom to top and left to right scan the cells of T until a black pixel, s, of P is found.
      Insert s in B.
      Set the current boundary point p to s i.e. p=s
      Let b = the pixel from which s was entered during the image scan.
      Set c to be the next clockwise pixel (from b) in M(p).
      While c not equal to s do
        If c is black
          insert c in B
          Let b = p
          Let p = c
          _(backtrack: move the current pixel c to the pixel from which p was entered)_
          Let c = next clockwise pixel (from b) in M(p).
        else
          _(advance the current pixel c to the next clockwise pixel in M(p) and update backtrack)_
          Let b = c
          Let c = next clockwise pixel (from b) in M(p).
        end If
      end While
    End

This algorithm worked great for finding just one object within a class (i.e., one human from a group of humans) but was quite slow to find the other objects. To speed up the process, I used the opencv find contours function to get the starting pixel of each of the objects present in the image. This way, the topology of the shapes were also preserved, as previously, holes within the shapes were not detected. An example of the output of the boundary extraction algorithm is show in Figure 2.

![fig02](/assets/images/boundary_00.png) *Figure 2. Boundary extracted using the Moore's Neighborhood Tracing algorithm.*

My next task was to explore skeleton pruning in order to simplify the skeleton obtained from the voronoi diagram constructed over the boundary points. Currently, I am taking individual objects and their skeletons, rather than pursuing all the objects in the image at once. The method I am following is as follows:
- Identify and remove the boundary edges, i.e. the edges that surround the shape. This will result in a skeletal representation of the shape that aren't bounded by edges.
- Next, I identify the valency of the vertices, i.e., find the number of edges attached to a given vertex. This step also helps me identify the edges that are connected to these edges.
- Vertices with valency 1 are the leaf nodes, i.e., vertices connected to only one edge, which occurs at the outermost branches of the skeletal representation.
- By iteratively removing edges containing the valency 1 vertices, I was able to get a simplified version of the object's skeleton. 
- The result of these steps can better be seen in Figure 3.

![fig03](/assets/images/fig3.png) *Figure 3. Skeleton pruning results across different number of iterations.*

To check the effect of the skeleton simplification on the shapes of the segmented images, I constructed voronoi diagrams taking the vertices on the spruned skeletons of the individual objects as the voronoi sites. The results can be seen in Figure 4.

![fig04](/assets/images/figure4.png) *Figure 4. Resegmented image by constructing Voronoi diagrams using skeleton vertices as the Voronoi sites.*

The issues that I am currently facing is that, some shapes are over-exaggerated in certain directions. For instance the human's head is getting greatly extended into the sky region as there is a lack of skeleton branches in the sky object that can prevent the extension. As such, I will have to carefully think of solutions to tackle this problem in a way that simplifies the image but also prevents over-exaggeration of shapes. 

Apart from this, my next goal will be to test out some other skeleton pruning techniques that can better capture the shape representation. I would also like to test out different meshing techniques withing these shapes to see how a simplified boundary can result in simplified meshes.


[Link to Home Page](https://sjvyas.github.io/csce645/)
