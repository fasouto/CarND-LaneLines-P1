# **Finding Lane Lines on the Road**

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline consist in the following steps in order:
 - Convert image to grayscale
 - Apply gaussian smoothing
 - Apply canny edge detection
 - Apply a mask with the shape of a polygon
 - Detect edges with hough algorithm
 - Consolidate all the lines on the image

To consolidate the lines and separate them into left and right lane, I modify the draw_lines() function. I simply calculate the slope of each line and separate them by the value of the slope into right and left. Then I simply calculate the mean of the slope and intercept of each lane, with this information I calculate the crossing point of the two lanes.

Each lane goes from the corner of the mask or the crossing point, whoever has the bigger value to the bottom of the image. With the slope, intercept and these two points draw the lane was trivial.

In order to approximate the lines I tried several methods, like use `numpy.polyval` and `scipy.optimize.curve_fit` to approximate the dots of each line to a polynomial. With this approach in theory the pipeline would be better in curves. The problem is that I should find a way to exclude outliers, because the function will try to pass trough all points making "funny" results.


### 2. Identify potential shortcomings with your current pipeline

While this approach is a good start I have the feeling that is not very usable in real-life environments. I had to tune several parameters by hand to find the correct value of the algorithms:

- What if it's snowing or foggy? Those parameters need to be adjusted properly.
What if there are some repairings on the road and/or traffic cones? This solution ignores them
- The mask method is not very reliable when we have a big uphill road in front of us. Works better with flat terrain.
- There could be lines in the road that indicate something else than lines, for example crossed lines that indicate an intersection that couldn't be blocked or a pedestrian crossing.
- I was tempted to mask yellow and white colors to make detection easier. But the markings could be red or multicolor and we want to detect every corner case.


### 3. Suggest possible improvements to your pipeline

Most of the image processing values are hardcoded, I don't think that is the propper way to do it. Some parameters should be adjusted in function of the weather, road brightness, time of the day...

We can use curve fitting methods to approximate better each lane. We are using straight lanes but the roads are not straight. The method ` scipy.optimize.curve_fit` could do the trick.
