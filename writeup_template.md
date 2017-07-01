# **Finding Lane Lines on the Road**

Date: June 30th

**Goal**

Generally speaking, the project is to automatically detect lane lines using an algorithm

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
    * color selection
    * region of interest selection
    * grayscaling
    * Gaussian smoothing
    * Canny Edge Detection
    * Hough Tranform line detection
* Apply the pipeline on an image
* Apply the pipeline to a video stream


[//]: # (Image References)

[image_a1]: ./writeup/before_draw_lines_changed/solidYellowLeft.jpg

[image_a2]: ./writeup/before_draw_lines_changed/gray.png "Grayscale"

[image_a3]: ./writeup/before_draw_lines_changed/blur_gray.png "Grayscale"

[image_a4]: ./writeup/before_draw_lines_changed/canny_edges.png "Grayscale"

[image_a5]: ./writeup/before_draw_lines_changed/masked_edges.png "Grayscale"

[image_a6]: ./writeup/before_draw_lines_changed/line_segments.png "Grayscale"

[image_a7]: ./writeup/before_draw_lines_changed/final_image.png "Grayscale"



---

### 1. Pipeline

My pipeline consisted of 5 steps.

1) First, I converted the images to grayscale.

    Here we read a .png and convert to 0,255 bytescale

    ![alt text][image_a1 | width=960]

    <img src=image_a1 width="48">

    Define a kernel size for Gaussian smoothing / blurring

    Define our parameters for Canny and run it

    reduce the region of interest

     Run Hough on edge detected image

     Define the Hough transform parameters

     Make a blank the same size as our image to draw on

     Output "lines" is an array containing endpoints of detected line segments



2) In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

### 2. Potential shortcomings

One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...

### 3. Possible improvements

A possible improvement would be to ...

Another potential improvement could be to ...
