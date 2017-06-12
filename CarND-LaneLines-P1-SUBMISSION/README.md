#**Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="laneLines_thirdPass.jpg" width="480" alt="Combined Image" />


## Writeup


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./output_test_images/output_solidWhiteCurve.jpg "solidWhiteCurve"
[image2]: ./output_test_images/output_solidWhiteRight.jpg "solidWhiteRight"
[image3]: ./output_test_images/output_solidYellowCurve.jpg "solidYellowCurve"
[image4]: ./output_test_images/output_solidYellowCurve2.jpg "solidYellowCurve2"
[image5]: ./output_test_images/output_solidYellowLeft.jpg "solidYellowLeft"
[image6]: ./output_test_images/output_whiteCarLaneSwitch.jpg "whiteCarLaneSwitch"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale then I blurred the 
image using Gaussian blur with kernel_size of 3. I then used canny transform to detect edges. The edges were then
filtered out to only include the regions where there is higher possiblity of finding lanes. This was done using
region_of_interest function. Finally, I used hough transform to find lanes and extrapolate those lanes from the 
edges. 

I had to modify the draw_lines() function by in-order to draw a single line on the left and right lanes. I 
accomplished this by first going over all the lines and classifying the line based on slope. If the slope
was negative, then the line belonged to right side of the lane and if the slope was positive then the line
was left side of the lane. I also filtered lines where the slope values did not make sense, such as, zero 
and inf slope. 


Here is the output from the running the code against test_images:
![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would happen when the camera angle changes. This was highly evident when I ran
my solution against the optional challenge video. I had to readjust the region_of_interest depending 
on the camera location and lanes. 

Another shortcoming could be when there are objects, shadows that look line lane markers. The algorithms 
is not very good at distinguishing actual lanes from safety railings. Both safety railings and solid yellow
left lane line marker look identical during hough line transform. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to have a dynamic way to determin the region_of_interest rather than 
having a pre-defined hardcoded way. 

Another potential improvement could be to have a better algorithm for extrapolation in the draw_line() function. 
Currently, the draw_line() function doesn't work well with curved lines. For example, instead of averaging the slope 
on the whole frame which results in 1 line; one could imporove by taking smaller sample sizes on the same frame
and extrapolate from those. This would result in multiple smaller lines which would over-impose the curved lines 
from the highway. 

