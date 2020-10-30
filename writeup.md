# Finding Lane Lines on the Road

## Overview

In this project, we wanted to define the lane that the driving car should stay in. This involved defining a pipeline that could take an image of the road from a car-mounted camera and ultimately annotate the image for the car to remain within the lane. We also added additional information for the car to use such as calculating the radius of curvature of the lane.

![Annotated image of road via the pipeline](images/example-lane_lines_complete.png)


# Pipeline Parts


## Pipeline Results: Video


## Camera Calibration (Distortion Correction)

Relevant code to define the camera matrix can be found in this notebook's section [finding_lanes-advanced.ipynb#Camera-Calibration](finding_lanes-advanced.ipynb#Camera-Calibration) and then applied in the pipeline with the function `undistort_image()` (found in this section [finding_lanes-advanced.ipynb#Undistort](finding_lanes-advanced.ipynb#Undistort)).

Below we use the camera matrix and distortion coefficients to transform the image into what we would expect the image to look like:

![Checkerboard comparison of distortion correction](images/example-distortion_correction.png)

We did this by first finding the camera matrix by assuming first that the chessboard image is fixed in the third dimension (z=0) so that the object points (corners of the squares within the grid) are the same for each chessboard calibration image. We then used the method from OpenCV `findChessboardCorners()` to find the corners of the board to create image points. Using OpenCV's `calibrateCamera()` method to find the camera calibration and distortion coefficients, we were then able to undistort images from the same camera via another OpenCV method `undistort()`.

This process was then part of the pipeline to undistort the images. A few sample images are shown below:

![Grid of undistorted image](images/example-distortion_correction-test_images.png)

## Color Transforms

We used a combination of applications of the OpenCV `Sobel()` method to get different attributes of the image gradient (each with its own tuned threshold and parameters.) This included calculating the absolute value of the x & y components of the gradient (`abs_sobel_thresh()`), the gradient magnitude (`mag_thresh()`), and the gradient direction (`dir_threshold()`).

These methods were tuned and combined together in the `full_binary_image()` method to produce images like the ones below:


![Several binary images after transform](images/example-binary_images.png)


## Perspective Transform

Relevant code that performed a perspective transformation (to a "bird's-eye view") can be see in this notebook's section [finding_lanes-advanced.ipynb#Perspective-Transform](finding_lanes-advanced.ipynb#Perspective-Transform). This was then incorporated into the overall pipeline. We defined a trapezoid that approximately aligns with lane lines from the road. We could then take these points to create a warp matrix which was then used to warp the image. Below are images before and after the transformation:

![Undistorted image with annotated lines of expected lane lines](images/example-perspective_transform-undistorted.png)
![Distorted "bird's eye" view of the previous image](images/example-perspective_transform-warped.png)


## Lane Line Pixels Identified with a Curved Functional Form

As the code in the notebook's section [finding_lanes-advanced.ipynb#Identify-Lane-Line-Pixels](finding_lanes-advanced.ipynb#Identify-Lane-Line-Pixels) shows, we use a 2nd order polynomial to estimate where the lane lines are after a perspective transformation. We start with using a sliding window to search for lane lines. Note we first use a histogram to find the initial guess for the first window and then using the results to define where to search next. We then took these results and visualized the estimation of the lane lines as shown in the image below (original image and perspective transformed image with annotated lane lines from the 2nd order polynomial estimation).

![Pair of images showing the lane line pixels being identified](images/example-rectified_lane_lines.png)


## Radius of Curvature Calculations

As can be seen in the next section of the notebook [finding_lanes-advanced.ipynb#Radius-of-Curvature-of-Lane-&-Position-of-Vehicle-w.r.t.-Center](finding_lanes-advanced.ipynb#Radius-of-Curvature-of-Lane-&-Position-of-Vehicle-w.r.t.-Center), we use the previous results of estimating the lane lines with a 2nd order polynomial to calculate the curvature of the lane. We assumed the camera sits at the center of the image. The pixel measurements were then converted to meters by knowing the approximate measurement values for the width of lanes and the space between lane line guides. We then superimposed this information onto the output image as shown below:

![Top left shows curvature calculations](images/example-radius_of_curvature.png)


# Discussion

## Challenges Faced

## Improvements
(TODO:)
- Where it fails
- Where it could fail
- Improvements to pipeline