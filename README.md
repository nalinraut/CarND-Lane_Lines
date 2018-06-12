# **Finding Lane Lines on the Road** 
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image0]: ./writeup/video.gif
[image1]: ./writeup/hsv_img.png
[image2]: ./writeup/gray_img.png
[image3]: ./writeup/gray_blur.png
[image4]: ./writeup/edges.png
[image5]: ./writeup/roi1.png
[image6]: ./writeup/extrapolated.png
[image7]: ./writeup/final_extrapolate.png
[image8]: ./writeup/image.jpg

---

### Reflection

### 1.The Pipeline
My pipelines consists of the following steps:

#### (i) Converting the input image to HLS from RGB: 
The <i>toHLS()</i> function converts the image to HLS scale and masks the image with yellow and white filters. The HLS       domain takes care of the variation in the lighting conditions.


![HLS Image][image1]


#### (ii) Converting the HSL masked image to grayscale:
The image is further converted into an grayscale image.

![Grayscale][image2]

#### (iii) Applying Guassian Blur:
Further, a Guassian Blur filter with kernel size 15 is used to remove noise.

![Gaussian Blur][image3]

#### (iv) Perform Canny Edge Detection:
Next the grayscaled image is fed into a canny edge detector and edges are obtained. A double threshold is used (low threshold = 100 and high threshold = 200) with low threshold to high threshold ratio between 1/2 p 1/3 as suggested.

![Canny][image4]

#### (v) Extract Region of Interest:
Edges are detected all over the image, however, the edges that matter are in a particular region therefore the polygonal region is extracted and only those edges that fall in that region are kept while rest are discarded.

![Canny with RoI][image5]

#### (vi) Draw Hough Lines on the Edge Image:
<div class=text-justify> The image with edges is used to draw Hough lines. The function basically converts the points in the image space to lines inthe Hough space. For drawing lines I use the <i>modified_draw_lines()</i> function to ensure several line segments are extrapolated/ averaged to one line segment per lane line, also ensuring the smoothness.
The raw line segments (more than one per lane line) are broken and need to be averaged as well as extrapolated and for this, I find the slope of each line segment and further classify the points on the line segments to be part of either positive slope or negative slope. Here the line segments with positive slope belong to the right lane line and line segments with negative slope belong to the left lane line. To fit a line, <i>polyfit()</i> with 1 degree is used and further the lane lines are drawn. A global list with a fixed length is maintained to which the coordinates are appended. This helps the algorithm to also keep track of previous points. This way, the broken lines can be extrapolated/ averaged to a single line.</div>

![Extrapolate][image6]

#### (vii) Extract Region of Interest:
Again, the extrapolated lane lines are drawn on the entire image. To avoid this a region of interest masks the image for the lane lines appear only in the region that matters.

![Extrapolate with RoI][image7]

#### (viii) Weighted Image:
The image with extrpolated lines goes on the original image and forms the final image.

![Final Image][image8]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
