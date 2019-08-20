Goal

In this tutorial you will learn how to:

a. Use the OpenCV function cv::remap to implement simple remapping routines.

Theory

What is remapping?

* It is the process of taking pixels from one place in the image and locating them in another position in a new image.
* To accomplish the mapping process, it might be necessary to do some interpolation for non-integer pixel locations, since there will not always be a one-to-one-pixel correspondence between source and destination images.
* We can express the remap for every pixel location (x,y) as:

g(x,y)=f(h(x,y))

where g() is the remapped image, f() the source image and h(x,y) is the mapping function that operates on (x,y).

* Let's think in a quick example. Imagine that we have an image I and, say, we want to do a remap such that:

h(x,y)=(I.cols-x,y)

What would happen? It is easily seen that the image would flip in the x direction. For instance, consider the input image:

![](https://docs.opencv.org/4.1.0/Remap_Tutorial_Theory_0.jpg)

observe how the red circle changes positions with respect to x (considering x the horizontal direction):

![](https://docs.opencv.org/4.1.0/Remap_Tutorial_Theory_1.jpg)

In OpenCV, the function cv::remap offers a simple remapping implementation.