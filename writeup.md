# **Finding Lane Lines on the Road** 
### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline consists of the following steps:
- First convert the image to grayscale.
- Apply the guassion blur filter.
- Detect edges using canny edge detection, I found low_thresold of 40 and high_threshold of 150 to work the best for the project.
- Get the region of interest using the image shape and assuming that the width of image is 40 pixels.
- Draw the hough lines and overlay them on the original image.

In order to draw the lines such that extroploate them, I seperated the left and right lines using the slope and I used 0.5 slope as the threshold such that only use lines below -0.5 and above 0.5 slope as others would be outliers being too much or too less at an angle. The right lane lines were with positive slope and left lanes were with negative slopes based on the coordinate system used in the image.

We know the start height and end height, such as y coordinate we want to draw line. Basically start is bottom of image being a 0,0 at top left, we take y1 as image.shape[0] . Lets draw upto 66% of the image, that's seems a reasonable number based on the camera angle and it's range of view, anything above 66% would be area above the lane/road in the image.

I used numpy polyfit function on the equation of line (y = mx + c) to find the coffiencients for the left lane and the right lane and determine the best line that fits the best of all those lines. As I know the start height and end height such as y1, y2 for the line and coffiecients were determined using the numpy polyfit, I dervied the x1 and x2 and plotted that line and same process was done for the right lane. 



### 2. Identify potential shortcomings with your current pipeline
The pitfalls for the pipeline is the kernel size and thresholds were best determined using multiple guesses and they might not work with some other lighting scenarios or situations. For example in the challenge problem the patch where the road patch gets lighter, the following pipeline doesn't work.
Addtionally the pipeline doesn't works great at turning lanes. Also, the width of the lane is assumed to be fixed to 40 pixels where it could vary in the real world.


### 3. Suggest possible improvements to your pipeline
To solve the challenge problem we require the mask the area of the image such that the region of interest only contains the road and not the surrounding. This can solved by knowing the exact range of view of cameras and mount position and width of the road.

Determining the width of the road dynamically and the region of interest based on threshold would help to scale the pipeline for broader use cases.

