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

[image_a1]: ./writeup/before_draw_lines_changed/original_image.png

[image_a2]: ./writeup/before_draw_lines_changed/gray.png "Grayscale"

[image_a3]: ./writeup/before_draw_lines_changed/blur_gray.png "Grayscale"

[image_a4]: ./writeup/before_draw_lines_changed/canny_edges.png "Grayscale"

[image_a5]: ./writeup/before_draw_lines_changed/masked_edges.png "Grayscale"

[image_a6]: ./writeup/before_draw_lines_changed/line_segments.png "Grayscale"

[image_a7]: ./writeup/before_draw_lines_changed/final_image.png "Grayscale"

[image_b6]: ./writeup/after_draw_lines_changed/line_segments.png "Grayscale"

[image_b7]: ./writeup/after_draw_lines_changed/final_image.png "Grayscale"

---

### 1. Pipeline

My pipeline consisted of 5 steps. Take the image, "./test_images/solidYellowLeft.jpg", shown as below as an example.

![alt text][image_a1]

1) I converted the images to grayscale.

    Here we read a .png and convert it to 0,255 bytescale

    ![alt text][image_a2]

2) I applied Gaussian smoothing / blurring on the gray image.

    A kernel size is needed to be defind for Gaussian smoothing / blurring.

    ![alt text][image_a3]

3) I computed the Canny edges for the blurred gray image.

    ![alt text][image_a4]

4) I reduced the region of interest by defining a triangle.

    ![alt text][image_a5]

5) I ran the Hough transformation on edge detected image and made a blank with the same size as the image to draw on. The output "lines" is an array containing endpoints of detected line segments

    ![alt text][image_a6]

6) Concatenate the line segments and the original image to generate a lined image.

    ![alt text][image_a7]

7) In order to draw a single line on the left and right lanes, I modified the draw_lines() function as shown below.

    Original:

    ```
    def draw_lines(img, lines, color=[255, 0, 0], thickness=2):
        for line in lines:
            for x1,y1,x2,y2 in line:
                cv2.line(img, (x1, y1), (x2, y2), color, thickness)
    ```

    Modified:

    ```
    def draw_lines(img, lines, color=[255, 0, 0], thickness=15):
        x_left = []
        y_left = []
        x_right = []
        y_right = []

        x_mid = img.shape[1] / 2

        for line in lines:
            for x1,y1,x2,y2 in line:
                slope = ((y2-y1)/(x2-x1))
                if abs(slope) > 0.4:
                    if x1 <= x_mid and x2 <= x_mid and slope < 0:
                        x_left.append(x1)
                        x_left.append(x2)
                        y_left.append(y1)
                        y_left.append(y2)
                    if x1 > x_mid and x2 > x_mid and slope > 0:
                        x_right.append(x1)
                        x_right.append(x2)
                        y_right.append(y1)
                        y_right.append(y2)

        #y_left
        if not x_left or not x_right:
            print("empty list...")
            return
        left_fit = np.polyfit(x_left, y_left,1)
        right_fit = np.polyfit(x_right, y_right,1)

        #Left lane
        m_left = left_fit[0]
        b_left = left_fit[1]

        minleft_y = img.shape[0]
        minleft_x = (minleft_y - b_left) / m_left

        maxleft_y = img.shape[0]*0.58
        maxleft_x = (maxleft_y - b_left) / m_left

        point_1x = int(minleft_x)
        point_1y = int(minleft_y)
        point_2x = int(maxleft_x)
        point_2y = int(maxleft_y)

        cv2.line(img, (point_1x, point_1y), (point_2x, point_2y), color,thickness)

        #Right lane
        m_right = right_fit[0]
        b_right = right_fit[1]

        minright_y = img.shape[0]*0.58
        minright_x = (minright_y - b_right) / m_right

        maxright_y = img.shape[0]
        maxright_x = (maxright_y - b_right) / m_right

        point_1xr = int(minright_x)
        point_1yr = int(minright_y)
        point_2xr = int(maxright_x)
        point_2yr = int(maxright_y)

        cv2.line(img, (point_1xr, point_1yr), (point_2xr, point_2yr), color,thickness)
    ```

    The idea is to separate each line segments into two lists, left and right, based on their slope and x coordinates.

    Then with extrapolation I computed the left slope and the right slope. Finally I got two lines representing the left lane and the right lane.

    After this modification, we obtained a new line image and a new final image as shown below.

    ![alt text][image_b6]

    ![alt text][image_b7]

### 2. Potential shortcomings

One potential shortcoming would be lane detection would fail when the lanes in the image are outside the area defined by the triangle.

Another shortcoming could be in my draw line function I sacrificed some images in which the line segments filtered into the left or list are none. As a result, in those images

### 3. Possible improvements

A possible improvement would be to define more dynamic area of interest according to the lanes.

Another potential improvement could be to find a better strategy to divide the line segments into two lists.
