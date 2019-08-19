目的

在这篇教程中，你将会学到：

* 使用OpenCV的HoughCircles()函数来检测图像中的圆圈。

理论

霍夫圆形变换

* 霍夫圆形变换和之前教程中所提及的霍夫线性变换相类似。
* 在线条检测案例中，线条使用的坐标是(r,θ)。在圆形检测中，我们用一下坐标来定义一个圆：

![](http://latex.codecogs.com/gif.latex?C:(x_{center},y_{center},r))

where ![](http://latex.codecogs.com/gif.latex?xcenter,ycenter) define the center position (green point) and r is the radius, which allows us to completely define a circle, as it can be seen below:

![](https://docs.opencv.org/4.1.0/Hough_Circle_Tutorial_Theory_0.jpg)

For sake of efficiency, OpenCV implements a detection method slightly trickier than the standard Hough Transform: The Hough gradient method, which is made up of two main stages. The first stage involves edge detection and finding the possible circle centers and the second stage finds the best radius for each candidate center. For more details, please check the book Learning OpenCV or your favorite Computer Vision bibliography

What does this program do?

* Loads an image and blur it to reduce the noise
* Applies the Hough Circle Transform to the blurred image .
* Display the detected circle in a window