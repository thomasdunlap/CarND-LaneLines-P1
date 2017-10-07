# **Finding Lane Lines on the Road** 

![original image][original] ![final image][weighted_lines]

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
[video]: ./test_videos_output/challenge.mp4 "Working Lane Lines Video"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

My pipeline has 5 major steps, which I've numbered both here and in the code for clarity:

#### Step 1: Highlight the Lanes Using Color Thresholds

First, we create a copy of our original image:

![original image][original]

Next, we use color thresholds to leave us with just the land line colors in the image: yellow and white. These red, green, and blue thresholds are mirrors of the pixel arrays in the original image, except they hold a boolean value of True or False instead of the pixel intensity.  Whether its true or false will be relative to the conditional statment in the thresholds (i.e., more than 220 for red). Then, for every threshold value that is "False," the equivalent pixel intesity in our copied image will be made equal to 0 (the color black).  Here is our image after thresholding:

![color select image][color_select]

Also, here is the grayscale version of our color_select image, which we will need in step two where we further highlight the lane lines:

![grayscale color select image][cs_gray]

#### 2. Identify and Highlight Edges 

We'll start by transforming the original image to grayscale: 

![grayscale image][gray]

Then we send a Gaussian smoother with a 9x9 kernal over ther image to reduce the edge noise. As you can see, this reduces the number of faint edges in our image, which we don't want averaged into our actually lane line data when the time comes:  

![blur gray image][blur_gray]

We run our blur_gray image through Canny Edge detection, which finds the edges based on the gradient of color change:

![canny edges image][edges]

From there, we run open cv's Morph-Closed function, where I chose a 5x5 kernal first dialates the edge lines (adds pixels of similar pixel intensity around edges) and then erodes (thins edge lines or deletes them if they aren't above a certain thickness.  This is helps us highlight and extend our lane line pieces:

![morphed edges image][edges_close]

Finally, we merge, or add, or grayscale color select (cs_gray) image with this edges_close image, to give even more emphasis to the lane line edges.  I used cv2.add() because any sumed value over the max of 255 is equal to 255, as opposed to other types of image addition that use modulus addition where, say 255 + 25 = 25. Here's the merged image:

![merged image][merged]

#### 3. Create region of interest and masked regions.

Our mask will turn everything outside our region of interest to black (pixel intensity of zero).  Through the very technical process of "trial and error," I eventually defined the points for regions of interest for the left lane, right lane, and both combined:

![masked image][masked]

#### 4. Used Hough Line Transformation on Region of Interest in Masked Image

Something about gradients and stuff!

![hough lines image][lines_img]

#### 5. Draw Lines and Add Them to Original Image With extrapolate_lines()

Finally, we grayscale our lines_img:

![grayscale hough lines image][gray_lines]

Next, we use the extrapolate_lines() function, which makes a best fit line across the right and left vertice points, finds their slope, and extrapolates lane lines based on highest and lowest points returned by the calculation:

![final lane lines image][final_lines]

Finally, we do a weighted add of our original image with slightly transparant overlay of our lane line images, and we have our final weighted images!

![final weighted lines image][weighted_lines]

#### Does This Transfer to Video?

In the words of a great philoserpher, "YOU'RE DARN TOOTIN' IT DOES!!":

![video][video]


### 2. Identify potential shortcomings with your current pipeline

There are potential shortcomings to the current pipeline.  The most obvious being this pipeline finds lane lines by averaging the the points directly in front of it.  A poorly painted road, debris, snow, water reflecting sunlight, and possibly even quickly changing angles of a very winding road could all confuse this mechanism.

One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

One obvious improvement would be averaging the lines in segments, instead of one bunch.  This should eleminate how the lane lines stick out into the road on curves, but could also become confusing if the road markings aren't clear or this is unexpected debris as mentioned above.  

A possible improvement would be to ...

Another potential improvement could be to ...
