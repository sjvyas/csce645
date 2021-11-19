title: Update 2

layout: default

permalink: /csce645/update-2

## Project Update 2

My main goals for this update were as follows:
- Implement a boundary tracing algorithm that works similarly to the boundary tracing algorithm in [MATLAB](https://www.mathworks.com/help/images/ref/bwboundaries.html)
- Explore skeleton pruning technique to improve the skeletal representation of the different objects present in the image.
- Preliminary exploration of reconstructing segmented images using the simplified skeleton.

My first step was to implement the boundary tracing algorithm. While quite a few boundary extraction functions exist in popular python libraries, such as [OpenCV](https://docs.opencv.org/3.4/d4/d73/tutorial_py_contours_begin.html) and [scikit-image](https://scikit-image.org/docs/dev/auto_examples/edges/plot_contours.html), they returned incomplete contours/boundaries (Figure 1). As such I implemented the Moore's neighborhood tracing algorithm, similar to the MATLAB function. Specifically I followed the following algorithm borrowed from [Wikipedia](https://en.wikipedia.org/wiki/Moore_neighborhood): 

```
**Input**: A square tessellation, T, containing a connected component P of black cells.
**Output**: A sequence B (b1, b2, ..., bk) of boundary pixels i.e. the contour.
Define M(a) to be the Moore neighborhood of pixel a.
Let p denote the current boundary pixel.
Let c denote the current pixel under consideration i.e. c is in M(p).
Let b denote the backtrack of c (i.e. neighbor pixel of p that was previously tested)
 
**Begin**
  **Set** B **to** be empty.
  **From** bottom **to** top **and** left **to** right scan the cells of T **until** a black pixel, s, of P is found.
  Insert s in B.
  **Set** the current boundary point p **to** s i.e. p=s
  **Let** b = the pixel from which s was entered during the image scan.
  **Set** c to be the next clockwise pixel (from b) in M(p).
  **While** c not equal to s do
    **If** c **is** black
      insert c in B
      **Let** b = p
      **Let** p = c
      _(backtrack: move the current pixel c to the pixel from which p was entered)_
      **Let** c = next clockwise pixel (from b) in M(p).
    **else**
      _(advance the current pixel c to the next clockwise pixel in M(p) and update backtrack)_
      **Let** b = c
      **Let** c = next clockwise pixel (from b) in M(p).
    **end If**
  **end While**
**End**
```

[Link to Home Page](https://sjvyas.github.io/csce645/)
