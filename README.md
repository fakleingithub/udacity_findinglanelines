# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[grayscaled]: ./test_images_output/grayscaled.jpg "grayscaled"
[gaussian_blur]: ./test_images_output/gaussian_blur.jpg "gaussian_blur"
[canny]: ./test_images_output/canny.jpg "canny"
[masked_edges]: ./test_images_output/masked_edges.jpg "masked_edges"
[edge_lines]: ./test_images_output/edge_lines.jpg "edge_lines"

[solidWhiteCurve]: ./test_images_output/solidWhiteCurve.jpg "solidWhiteCurve"
[solidWhiteRight]: ./test_images_output/solidWhiteRight.jpg "solidWhiteRight"
[solidYellowCurve]: ./test_images_output/solidYellowCurve.jpg "solidYellowCurve"
[solidYellowCurve2]: ./test_images_output/solidYellowCurve2.jpg "solidYellowCurve2"
[solidYellowLeft]: ./test_images_output/solidYellowLeft.jpg "solidYellowLeft"
[whiteCarLaneSwitch]: ./test_images_output/whiteCarLaneSwitch.jpg "whiteCarLaneSwitch"

---

### Reflection

### 1. Description of the lane lines finding pipeline

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied a gaussian blur on the images with a kernel size of 3. After that I applied the canny edge detection with a low threshold of 50 and a high threshold of 150. To set a region of interest for the lane lines I defined vertices and applied an image mask. In order to apply the hough line transformation I defined all parameters for the function. When the hough line transformation function is applied it detects the line segments and draws seperate lines on it.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by calculating the slope and intercept of every seperate line. To filter out outliers of slopes which would impact the mean values, slopes with an absolute higher than 0.9 and lower than 0.1 and the related intercepts are not used in the next steps. All other slopes with a positive value will be appended in a list and also the relating intercept will be stored in a list. These two lists are related to left lane lines, because they have a positive slope. Then there are also two lists for the right lane lines with negative slope and the relating intercept. 
I calculate the mean of the two slope and two intercept lists to get the average slope and intercept for the left lane line and the right lane line. 
Then the start- and endpoint of the left lane line and right lane line will be calculated with the help of the linear equation, the highest and lowest y-value in the region of interest and the average slope and intercept.

With the weighted_img function the two calculated lane lines are projected onto the original image.

Here you see the different steps of the pipeline:
![alt text][grayscaled] ![alt text][gaussian_blur] ![alt text][canny] 
![alt text][masked_edges] ![alt text][edge_lines]

Here are my results for the test images provided:

![alt text][solidWhiteCurve] ![alt text][solidWhiteRight] ![alt text][solidYellowCurve] 
![alt text][solidYellowCurve2] ![alt text][solidYellowLeft] ![alt text][whiteCarLaneSwitch] 


### 2. Potential shortcomings 


One potential shortcoming would be what would happen when there are stronger curves within the region of interest. The average slope would change quickly and a straight line could not describe these curved lines. The projected lane line would change quickly with a high difference in slope value.

Another shortcoming could be if there are difficult lightning conditions because of tree- or building-shadows, bad weather, evening or night scenes. Then the functions of the canny detection and hough lines would not work properly anymore and not enough lines or wrong lines would be detected.

Also its unclear what happens when lane lines are partly missing or a car in front of the ego vehicle changes lanes.


### 3. Possible improvements 

A possible improvement would be to have an adaptive region of interest, so the region gets smaller stronger curves.

Another potential improvement could be to adapt the parameters for canny-detection and hough lines dynamically according to the lightning conditions.
