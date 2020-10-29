# Finding Lane Lines on the Road

## Overview

In this project, we wanted to define the lane that the driving car should stay in. This involved defining a pipeline that could take an image of the road from a car-mounted camera and ultimately annotate the image for the car to remain within the lane. We also added additional information for the car to use such as calculating the radius of curvature of the lane.

![Annotated image of road via the pipeline](images/example-lane_lines_complete.png)


# Pipeline Parts


## Pipeline Results: Video


## Camera Calibration (Distortion Correction)

![Checkerboard comparison of distortion correction](images/example-distortion_correction.png)


## Color Transforms

![Several binary images after transform](images/example-binary_images.png)


## Perspective Transform

![Undistorted image with annotated lines of expected lane lines](images/example-perspective_transform-undistorted.png)
![Distorted "bird's eye" view of the previous image](images/example-perspective_transform-warped.png)


## Lane Line Pixels Identified with a Curved Functional Form

![Pair of images showing the lane line pixels being identified](images/example-rectified_lane_lines.png)


## Radius of Curvature Calculations

![Top left shows curvature calculations](images/example-radius_of_curvature.png)



# Discussion

## Challenges Faced

## Improvements
(TODO:)
- Where it fails
- Where it could fail
- Improvements to pipeline