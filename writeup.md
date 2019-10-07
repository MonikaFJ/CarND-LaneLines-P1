# **Finding Lane Lines on the Road**

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/after_first_step.jpg "Results after first step"
[image2]: ./examples/final_result.jpg "Final result"



### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 2 basic parts:

#### The first one is working with image, the input is image read from the disk and the output is detected lines within region if interest.

1. Convert to grayscale to reduce the required computational power
2. Apply gaussian blur to reduce noise
3. Apply Canny edge detector to extract edges
4. Apply region of interest to reduce the required computational power and ignore edges that are outside region of interest.
5. With Hugh Transformation join edges in lines

![alt text][image1]

#### The second part is working on detected lines. And as a result creates two lines that can be overlayed on the image.

1. Sort lines to lines that can represent left and right line depending on their angle (it could happen that line does not belong to any group)
2. From start and end points in the lines estimate the slope and intercept of the line that fits them best
3. With the information about y coordinates of the lines (taken from region of interest) the coordinates of the beginning and the end of the lines are calculated

Last step is to overlay original image with detected lines with draw_lines() function

![alt text][image2]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the car is not ideally positioned between the lines. The region of interest is right now set in a way that it might filter out the lines. Moreover it is not working good with shadows and bad lighting.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to add running average to the position of the lines. The output would be smoother.
Also right now the corners of the roi are calculated everytime based on the image size. If the assumption can be made that all images are of the same size this calculation can be done once.

---

## Notes from videos

### General vision

* [0,0] top left
* RBB -> 0 - color there 255 - color not there (8 bit - 2⁸)

### Canny edge detector

What would make sense as a reasonable range for these parameters? In our case, converting to grayscale has left us with an 8-bit image, so each pixel can take 2^8 = 256 possible values. Hence, the pixel values range from 0 to 255.

This range implies that derivatives (essentially, the value differences from pixel to pixel) will be on the scale of tens or hundreds. So, a reasonable range for your threshold parameters would also be in the tens to hundreds.

As far as a ratio of low_threshold to high_threshold, John Canny himself recommended a low to high ratio of 1:2 or 1:3.

### Hugh space

In Hugh space, I can represent my "x vs. y" line as a point in "m vs. b" instead. The Hough Transform is just the conversion from image space to Hough space. So, the characterization of a line in image space will be a single point at the position (m, b) in Hough space.

In Hugh space line is represented as point and point is represented as line. By finding the line that are intersecting in Hugh space, one can find points that creates a line on the image.

The simplest case of Hough transform is detecting straight lines. In general, the straight line y = mx + b can be represented as a point (b, m) in the parameter space. However, vertical lines pose a problem. They would give rise to unbounded values of the slope parameter m. Thus, for computational reasons, Duda and Hart[5] proposed the use of the Hesse normal form

    r = x cos ⁡ θ + y sin ⁡ θ

where r is the distance from the origin to the closest point on the straight line, and θ  is the angle between the x axis and the line connecting the origin with that closest point.  [from wiki]

### Parameter tuning

https://medium.com/@maunesh/finding-the-right-parameters-for-your-computer-vision-algorithm-d55643b6f954

---
