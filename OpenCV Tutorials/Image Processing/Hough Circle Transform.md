Goal

In this tutorial you will learn how to:

* Use the OpenCV function HoughCircles() to detect circles in an image.

Theory

Hough Circle Transform

* The Hough Circle Transform works in a roughly analogous way to the Hough Line Transform explained in the previous tutorial.
* In the line detection case, a line was defined by two parameters (r,θ). In the circle case, we need three parameters to define a circle:

![](http://latex.codecogs.com/gif.latex?C:(x_{center},y_{center},r))

where ![](http://latex.codecogs.com/gif.latex?xcenter,ycenter) define the center position (green point) and r is the radius, which allows us to completely define a circle, as it can be seen below:

![](https://docs.opencv.org/4.1.0/Hough_Circle_Tutorial_Theory_0.jpg)

For sake of efficiency, OpenCV implements a detection method slightly trickier than the standard Hough Transform: The Hough gradient method, which is made up of two main stages. The first stage involves edge detection and finding the possible circle centers and the second stage finds the best radius for each candidate center. For more details, please check the book Learning OpenCV or your favorite Computer Vision bibliography

What does this program do?

* Loads an image and blur it to reduce the noise
* Applies the Hough Circle Transform to the blurred image .
* Display the detected circle in a window