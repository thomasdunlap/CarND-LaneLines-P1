# **Finding Lane Lines on the Road** 

![original image][original] ![final image][weighted_lines]

---

**Finding Lane Lines on the Road**

The goals of this project are the following:
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

### Data Pipeline

My pipeline has 5 major steps, which I've numbered both here and in the code:

#### Step 1: Highlight the Lanes Lines Using Color Thresholds

First, we create a copy of our original image:

![original image][original]

Next, we use color thresholds to highlight just the lane line colors: yellow and white. The red, green, and blue thresholds are mirrors of the pixel arrays in the original image, except that they hold a boolean values of True or False instead of pixel intensity.  True and False are by the conditional statment in the thresholds (i.e., more than 220 for red). For every threshold value that is "False," the equivalent pixel intesity in our image will be made black (equal to zero).  Here is the image after thresholding:

![color select image][color_select]

Here is the grayscale version of our color_select image, which we will need in Step 2 to further highlight the lane lines:

![grayscale color select image][cs_gray]

#### Step 2: Identify and Highlight Edges 

We'll start by transforming the original image to grayscale: 

![grayscale image][gray]

Then we use a Gaussian smoother with a 9x9 kernal to reduce the edge noise. As you can see, this reduces the number of faint edges in our image, which we don't want averaged into our lane line data in future steps:  

![blur gray image][blur_gray]

The blur_gray image is run through Canny Edge detection, which finds the edges based on the steepest gradient pixel instensity change  -- or where two distinct colors meet for an extended period of time:

![canny edges image][edges]

From there, we run OpenCv's Morph-Closed function, where I chose a 5x5 kernal which first dialates the edge lines (adds pixels of similar pixel intensity around edges) and then erodes (thins edge lines or deletes them if they aren't above a certain thickness.  This is helps us highlight and extend our Canny Edges:

![morphed edges image][edges_close]

Finally, we merge, or add, or grayscale color select (cs_gray) image with this edges_close image, to give even more emphasis to the lane line edges.  I used cv2.add() because any sumed value over the max of 255 is equal to 255, as opposed to other types of image addition that use modulus addition where, say 255 + 25 = 25. Here's the merged image:

![merged image][merged]

#### Step 3: Create a Region of Interest and Masked Region.

Our mask will turn everything outside our region of interest to black (pixel intensity of zero).  Through the very technical process of "trial and error," I eventually defined the points for regions of interest for the left lane, right lane, and both combined:

![masked image][masked]

#### Step 4: Used Hough Line Transformation on Region of Interest in Masked Image

Hough lines are lines based on polar cooridnates. You can finde edges from the interception two Hough lines.  The more intersections, the more defined the line is:

![hough lines image][lines_img]

#### Step 5: Draw Lines and Add Them to Original Image With extrapolate_lines()

We need to  grayscale our lines_img for our extrapolate_lines() function:

![grayscale hough lines image][gray_lines]

The extrapolate_lines() function creates two lane lines by making a best fit line across the right and left vertices' points, finding their slope, and extrapolating the final lane lines based on highest and lowest points returned by that calculation:

![final lane lines image][final_lines]

Finally, we do a weighted add of our original image with slightly transparant overlay of our lane line images, and we have our final weighted images!

![final weighted lines image][weighted_lines]


### Potential Shortcomings of Current Pipeline

There are a few potential shortcomings with my current pipeline.  The most obvious being that the final lane lines are found with an averaged best fit line it. A dirt road, poorly painted lines, or debris could all throw off the lane lines.  You can see the beginnings of that alread in the wobble of the extrapolated lines over a dotted lane line.   

Similiarly, color could confuse it. Snow, puddles reflecting sunlight, or even the wrong color car directly in front. It would get confused by words written in white on the street, like school zone. 

Also, having an obstructed view, or being on the crest of a hill where the camera was seeing more horrizon than road could be a problem as well.

Finally, if someone were to put the camera at a different angle, or there was low contrast, it would be unlikely to reproduce these results.



### Possible Improvements

One improvement would be extrapolating the lines into small, connected segments, instead of one long line.  This should eliminate how the lane lines stick out into the road on curves, but could also become confusing if the road markings aren't clear or there is unexpected debris as mentioned above.  

Another improvement would involve being able to identify the edges of the road, cars, objects that are not lines using object identification.

A third improvement would be having different programs for different weather.  In the snow you can't see lane lines, but you can often make out tire tracks and you can approximate where the road is relative to what's its sides.

