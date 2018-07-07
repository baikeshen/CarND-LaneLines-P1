# **Finding Lane Lines on the Road** 



**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### *My solution for this project can be found in the Jupyter notebook [here](https://github.com/baikeshen/CarND-LaneLines-P1/blob/master/P1.ipynb)*



### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.
First, I converted the images to grayscale, then I used gaussian blur to make the image soft so to decrease some noises.

Then I use Canny Edge Detection to get the edges in images. To get each line, I use Hough Transfer to connect each edges dot.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by

Calculate slope of each line and separate by right lines (plus slope) and left lines(minus slope)
Eliminate over tilted slope
Make x range for right/left lines, eliminate out of range lines
Calculated average x and y position of right and left line, deleted too far away points/lines
Sorted by x value with descendant order and screen out over bounding points/lines
In order to reduce jitter of lines, I further have following strategy:

After above screening process, re-calculated average x and y position of right and left line. I memorized every average x and y position and average each frame of movie. To avoid sometime, there are too few lines in some occasion, I will check average x and y position is not too far away from history-average x/y position.
Memorized every average slope of left/right lines. Eliminate some over tilted average slope in some too few lines occasion.
My strategy to draw the single final left/right line:

Try to find the maximum 2 x value points and average them to be a start right point. Minimum 2 x value points to be a start left point.
If the start point of left/right line is too high for some dot line occasion, I will extrapolate it with history average slope
End point of a single line is average x and y position of each point of left/right line. I will also filter out some average point that is too far away from history average point.

![alt text][image1]


It was also successfully tested on the provided videos: white and yellow (click to watch).

[![White Basic](http://img.youtube.com/vi/pvyCv6XrOyY/0.jpg)](https://youtu.be/Cd1qrCZvP9c "Self-Driving Car Nanodegree - P1: Finding Lane Lines - White Basic")


[![Yellow Basic](http://img.youtube.com/vi/qAYoS7JaMnk/0.jpg)](https://youtu.be/WXSAXp6eKEY "Self-Driving Car Nanodegree - P1: Finding Lane Lines - Yellow Basic")

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be a little shift of final single start point when there are too many horizontal lines in the bottom of image.

Another shortcoming could be the mask range is fixed and can not fit into every scenario such as Optional Challenge movie.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to further average the start point of each frame and screen out the shift.

Another potential improvement could be to use start point with average x and y position then to extrapolate the line with history average slope of each frame.


