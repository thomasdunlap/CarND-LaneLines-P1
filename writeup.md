# **Finding Lane Lines on the Road** 

![original image][original] ![final image][weighted_lines]

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report
* Put extra comments into code to make it easier to understand

[//]: # (Image References)

[blur_gray]: ./examples/blur_gray.jpg "Blur Gray"
[color_select]: ./examples/color_select.jpg "Color Select"
[cs_gray]: ./examples/cs_gray.jpg "Grayscale Color Select"
[edges]: ./examples/edges.jpg "Canny Edges"
[edges_close]: ./examples/edges_close.jpg "Morph Closed Edges"
[final_lines]: ./examples/final_lines.jpg "Final Lines"
[gray]: ./examples/gray.jpg "Grayscale Original"
[gray_lines]: ./examples/gray_lines.jpg "Gray Lane Lines"
[lines_img]: ./examples/lines_img.jpg "Hough Lines"
[masked]: ./examples/masked.jpg "Masked Image"
[merged]: ./examples/merged.jpg "Merged Image"
[original]: ./examples/original.jpg "Original Image"
[weighted_lines]: ./examples/weighted_lines.jpg "Weighted Result Image"


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

My pipeline has 5 major steps. The first is creating the color_select image, to highlight the white and yellow lines from the original image. We start by copying the original image.

![original image][original]

The creating thresholds that 

![color select image][color_select]

Put selection in grayscale to further highlight? for future processing.

![grayscale color select image][cs_gray]

Next, we identify and highlight the edges of the lines in the image, starting by transforming the original image to grayscale: 

![grayscale image][gray]

Then we send a Gaussian smoother with a 9x9 kernal over ther image, to reduce the edge noise.

![blur gray image][blur_gray]

We run our Blur Gray image through Canny Edge detection, which finds the edges based on the gradient of color change or some shit.

![canny edges image][edges]

From there, we run open cv's morph-closed function, where I chose a 5x5 kernal first dialates the edges (adds pixels of similar pixel intensity around edges) and then erodes them.  This is helps us highlight and extend our lane line pieces a bit.

![morphed edges image][edges_closed]

Finally for step 2, we merge, or add, or Grayscale Color Select image with this edged image, to give even more emphasis to the lane line edges.

![merged image][merged]

3. Create region of interest and masked regions.

![masked image][masked]

4. Used Hough Line Transformation on Region of Insterest in masked image

![hough lines image][lines_img]

5. 

Make hough lines grayscale

![grayscale hough lines image][gray_lines]

Use final lane lines function, which takes the average slopes of the lines in an image, finds their slope, and extrapolates the line.

![final lane lines image][final_lines]



![final weighted lines image][weighted_lines]



### 2. Identify potential shortcomings with your current pipeline

There are potential shortcomings to the current pipeline.  The most obvious being this pipeline finds lane lines by averaging the the points directly in front of it.  A poorly painted road, debris, snow, water reflecting sunlight, and possibly even quickly changing angles of a very winding road could all confuse this mechanism.

One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

One obvious improvement would be averaging the lines in segments, instead of one bunch.  This should eleminate how the lane lines stick out into the road on curves, but could also become confusing if the road markings aren't clear or this is unexpected debris as mentioned above.  

A possible improvement would be to ...

Another potential improvement could be to ...
