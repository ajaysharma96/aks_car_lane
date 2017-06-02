# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)
[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images/output_greysolidWhiteCurve.jpg 
[image3]: ./test_images/output_blur_greysolidWhiteCurve.jpg 
[image4]: ./test_images/output_canny_edgesolidWhiteCurve.jpg 
[image5]: ./test_images/output_maskedsolidWhiteCurve.jpg 
[image6]: ./test_images/output_hough_linesolidWhiteCurve.jpg 
[image7]: ./test_images/output_weightedsolidWhiteCurve.jpg 
[image8]: ./test_images/output_draw_linesolidWhiteCurve.jpg 
[video1]: ./test_videos/output_solidWhiteRight.mp4 

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

1) First, I converted the images to grayscale,  
![alt text][image2]
2) Then I used the gaussian blur to smoothen out the edges 
![alt text][image3]
3) Then I used the canny function to get the edges
![alt text][image4]
4) Then I created a masked image to isolate the region of interest
![alt text][image5]
5) Then I used the hough lines to carve out the edges
   In order to draw a single line on the left and right lanes, I modified the draw_lines() function as follows:
- First divide the points based on which slope they fall into. We are basically looking for a positive slope and a negative slope.
Also the slope needs to meet a minimum requirement of being in the 0.4 to 0.9 . Any other lines that fall out of this slope are discarded.
- Then we calculate the average points for the two slopes - left slope and right slope as shown in the image below
![alt text][image8]
- Now we extrapolate the lines using the formula delta_x = delta_y / slope . We know the upper and lower boundaries are fixed (y axis) for the final region , so using those end points we extend the current slopes to get the final plots as below
![alt text][image6]

Finally used the weighted function to superimpose the carved out region of interest image on the actual image 
![alt text][image7]

![alt text][video1]

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when we are dealing with curved lines. We might have to use more sophisticated functions such as polyfit to get the proper curvature fitting in atleast 3 cubic degrees. 

Another shortcoming could be the processing time. This algorithm is rudimentary . There is lot of scope for improvement [caching, using more sophisticated python libraries]


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to cache the pipeline if there is not much change in the curvature or linearity.

