# **Finding Lane Lines on the Road Writeup**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[original]: /test_images/solidWhiteRight.jpg
---

## Reflection

### 1. Pipeline Description

My pipeline consisted of 6 steps:

Convert the image to grayscale to obtain a single channel pixel value range of [0, 255] and applying a Gaussian Blur to smooth the image and suppress noise.

[Gaussian Blur]: /test_images_output/blur.jpg

Next we apply Canny Edge detector based on optimal threshold values to detect the strong gradient along the image. This allows us extract sharp changes in values from the image such as bright lines on a black road.

[Canny]: /test_images_output/canny.jpg

Now that we have extracted these values we can mask the image to obtain the values within the desired region of interest defined by a polygon. The ROI is an approximate field of vision that extends to the horizon.

[ROI]: /test_images_output/region.jpg

The next step is applying the Hough Transform to the ROI in order to extract lines given parameters for number of intersections, minimum line length, and maximum line gap. Once this lines are extracted, they are fed to a draw lines function that produces the desired lines on the road.

[Hough]: ./test_images_output/hough_lines.jpg

The draw lines function was modified to filter the lines obtained from the Hough Transform to generate left and right lanes. Lines are filtered by slope and then their position is averaged to produce a model for left and right lanes. Using these, we can extrapolate to the desired endpoints in the ROI and produce two singe lines corresponding to the left and right lanes. To produce a better result for video processing, the previous frame values are used to average new computed averages.

[Weighted]: /test_images_output/weighted.jpg

The last step involves imposing the obtained lanes to the original image generating a frame with two marked lanes.

[Averaging]: /test_images_output/solidWhiteRight.jpg

### 2. Potential Shortcomings with Current Pipeline


One shortcoming of the current approach is that the pipeline is not optimized to work with a larger set of images and is therefore bound to fail under certain images if noise is not suppressed. Also, having a fixed ROI restricts frame processing to inadequate regions where lanes may not be present.  

### 3. Possible Improvements to Pipeline

One possible improvement is to introduce more filtering steps to images and introducing a fail safe mechanism for computations producing outliers. Also, using a larger data set can be used to optimize parameters to work for wide range of images.
