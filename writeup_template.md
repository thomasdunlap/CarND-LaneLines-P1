# **Finding Lane Lines on the Road** 

![alt text][image2]

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./examples/original.jpg "Original Image"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

There are potential shortcomings to the current pipeline.  The most obvious being this pipeline finds lane lines by averaging the the points directly in front of it.  A poorly painted road, debris, snow, water reflecting sunlight, and possibly even quickly changing angles of a very winding road could all confuse this mechanism.

One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

One obvious improvement would be averaging the lines in segments, instead of one bunch.  This should eleminate how the lane lines stick out into the road on curves, but could also become confusing if the road markings aren't clear or this is unexpected debris as mentioned above.  

A possible improvement would be to ...

Another potential improvement could be to ...
