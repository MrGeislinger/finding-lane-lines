# **Finding Lane Lines on the Road** 

## Reflection

### Pipeline Description

My pipeline to find lane lines consisted of a few steps. First, I used OpenCV to 
find the elements that were in a specified color range of yellow or white. Next,
I took this image and applied a Gaussian blur to help with the next step, edge 
detecting. For edge detecting, a lower & upper threshold are given to perform 
the Canny edge detection algorithm. This gives a good start on creating edges so
the next step was to create the lane lines.

First, I created an area of interest (trapezoid) that looked like it would 
contain the lane lines in the image. This was based on the sample images and 
where other cars in the sample images/videos were located. Next, Hough lines 
were found after playing with the parameters that gave reasonable results. 
Lastly, to create the lane line guides, the Hough lines were split into 
negative-sloped lines, positive sloped-lines, and lines outside of a given 
threshold. Only the negative and positive slope lines were used. The median 
slope & point in x (at the bottom of the image) was then found for each lane. 
These were then used to extrapolate and drawn in blue with the `draw_lines()`
function.

These steps produce an image with just lane line guides, so we then combine this
image with the original image so that it looks something like this below:

![](test_images_output/solidYellowCurve-after.jpg)



###  Shortcomings

The pipeline has some issues when there are sudden changes in color in the image
such as shadows or abrupt changes in asphalt color. This clearest in the 
_challenge.mp4_ video stream.

The pipeline has some issues when lane segments (broken lines) are far away.
This is due to the Hough line parameters restricting the size (minimum line 
length and maximum line gap). So the pipeline won't be able to detect these 
lines well until they're nearer to the vehicle (camera).


### Improvements

The pipeline definitely has room for improvement. For one, more parameter tuning
on the Canny algorithm and Hough lines could improve the edge detection. This
can be a very time consuming process especially when trying to expand the 
results to be more general.

Another very clear potential improvement to the pipeline would be to extrapolate
between frames in a video stream. Each frame is treated as an independent image
and it obviously has a difficult time if one or more frames are difficult in
lane detection. One solution would be to use a rolling average of previous 
lane guides. This would help prevent lane guides jumping around or suddenly 
disapperaing in a video stream output.