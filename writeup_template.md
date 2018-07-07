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

When applying my pipeline to the challenge video. It does not work so well. All these shortcomings render my pipeline unable to properly process and obtain annotations for the challenge video (click to play).

[![Challenge Basic](http://img.youtube.com/vi/GubKeWtd768/0.jpg)](https://youtu.be/Slz_H4X-YMM "Self-Driving Car Nanodegree - P1: Finding Lane Lines - Challenge Basic")

The challenges are due to the following reasons:

 a) It is hard to identify the yellow lines if they are not different from the ground
 
 b) There are a lot of curved lines rather than straight lines in the challenge video
 
 c) Variation of road surface adversly impacts the detection of yellow line and white line
 
 d) My mask range is fixed and can not fit into every scenario.


### 3. Suggest possible improvements to your pipeline

potential improvements:
1) Grayscale intensity thresholding was a good alternative for detecting white lanes and it even worked well for yellow lanes on the test image. However, for more challenging scenarios as the example we are dealing with, it turns out to be not enough, A more accurate thresholding for a bi-clor masking could be used.
2) Applying outlier removing to prune the outlier lines. A slope-based filter could be applied to filter out any lines whose slope is too big or too small.



