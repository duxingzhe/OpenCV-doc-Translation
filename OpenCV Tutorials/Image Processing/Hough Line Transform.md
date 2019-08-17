目标

在本篇教程中，你将会学到：

* 使用OpenCV的HoughLines()和HoughLinesP()来检测图像的线条。

理论介绍

> 注意
>
> 这些例子来自Bradski和Kaehler所著的Learning OpenCV一书。

霍夫线条变换

1.霍夫线条变换是用于检测直线的。

2.To apply the Transform, first an edge detection pre-processing is desirable.

How does it work?

1.As you know, a line in the image space can be expressed with two variables. For example:

    a.In the Cartesian coordinate system: Parameters: (m,b).
    b.In the Polar coordinate system: Parameters: (r,θ)

![](https://docs.opencv.org/4.1.0/Hough_Lines_Tutorial_Theory_0.jpg)

For Hough Transforms, we will express lines in the Polar system. Hence, a line equation can be written as:

![](http://latex.codecogs.com/gif.latex?y%20%3D%20%5Cleft%20%28%20-%5Cdfrac%7B%5Ccos%20%5Ctheta%7D%7B%5Csin%20%5Ctheta%7D%20%5Cright%20%29%20x%20+%20%5Cleft%20%28%20%5Cdfrac%7Br%7D%7B%5Csin%20%5Ctheta%7D%20%5Cright%20%29)

Arranging the terms: r=xcosθ+ysinθ

1.In general for each point ![](http://latex.codecogs.com/gif.latex?%28x_%7B0%7D%2C%20y_%7B0%7D%29), we can define the family of lines that goes through that point as:

![](http://latex.codecogs.com/gif.latex?r_%7B%5Ctheta%7D%20%3D%20x_%7B0%7D%20%5Ccdot%20%5Ccos%20%5Ctheta%20+%20y_%7B0%7D%20%5Ccdot%20%5Csin%20%5Ctheta)

Meaning that each pair ![](http://latex.codecogs.com/gif.latex?%28r_%7B%5Ctheta%7D%2C%5Ctheta%29) represents each line that passes by ![](http://latex.codecogs.com/gif.latex?%28x_%7B0%7D%2C%20y_%7B0%7D%29).

2.If for a given ![](http://latex.codecogs.com/gif.latex?%28x_%7B0%7D%2C%20y_%7B0%7D%29) we plot the family of lines that goes through it, we get a sinusoid. For instance, for x0=8 and y0=6 we get the following plot (in a plane θ - r):

![](https://docs.opencv.org/4.1.0/Hough_Lines_Tutorial_Theory_1.jpg)

We consider only points such that r>0 and 0<θ<2π.

3.We can do the same operation above for all the points in an image. If the curves of two different points intersect in the plane θ - r, that means that both points belong to a same line. For instance, following with the example above and drawing the plot for two more points: ![](http://latex.codecogs.com/gif.latex?x_%7B1%7D%20%3D%204), ![](http://latex.codecogs.com/gif.latex?y_%7B1%7D%20%3D%209) and ![](http://latex.codecogs.com/gif.latex?x_%7B2%7D%20%3D%2012), ![](http://latex.codecogs.com/gif.latex?y_%7B2%7D%20%3D%203), we get:

![](https://docs.opencv.org/4.1.0/Hough_Lines_Tutorial_Theory_2.jpg)

The three plots intersect in one single point (0.925,9.6), these coordinates are the parameters ( θ,r) or the line in which ![](http://latex.codecogs.com/gif.latex?%28x_%7B0%7D%2C%20y_%7B0%7D%29), ![](http://latex.codecogs.com/gif.latex?%28x_%7B1%7D%2C%20y_%7B1%7D%29) and ![](http://latex.codecogs.com/gif.latex?%28x_%7B2%7D%2C%20y_%7B2%7D%29) lay.

4.What does all the stuff above mean? It means that in general, a line can be detected by finding the number of intersections between curves.The more curves intersecting means that the line represented by that intersection have more points. In general, we can define a threshold of the minimum number of intersections needed to detect a line.

5.This is what the Hough Line Transform does. It keeps track of the intersection between curves of every point in the image. If the number of intersections is above some threshold, then it declares it as a line with the parameters ![](http://latex.codecogs.com/gif.latex?%28%5Ctheta%2C%20r_%7B%5Ctheta%7D%29) of the intersection point.

Standard and Probabilistic Hough Line Transform

OpenCV implements two kind of Hough Line Transforms:

a. The Standard Hough Transform

* It consists in pretty much what we just explained in the previous section. It gives you as result a vector of couples ![](http://latex.codecogs.com/gif.latex?%28%5Ctheta%2C%20r_%7B%5Ctheta%7D%29)
* In OpenCV it is implemented with the function HoughLines()

b. The Probabilistic Hough Line Transform

* A more efficient implementation of the Hough Line Transform. It gives as output the extremes of the detected lines ![](http://latex.codecogs.com/gif.latex?%28x_%7B0%7D%2C%20y_%7B0%7D%2C%20x_%7B1%7D%2C%20y_%7B1%7D%29)
* In OpenCV it is implemented with the function HoughLinesP()

What does this program do?

* Loads an image
* Applies a Standard Hough Line Transform and a Probabilistic Line Transform.
* Display the original image and the detected line in three windows.