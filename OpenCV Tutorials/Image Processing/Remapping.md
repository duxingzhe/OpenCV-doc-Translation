目的

在这篇教程中，你将会学到：

a. 使用OpenCV的cv::remap函数实现简单的重映射路径。

理论

什么叫重映射？

* 这是将图像中取出一个像素，并对应到另外一张新图片的对应位置。
* 为了能完成这种映射过程，我们需要对非整型像素区域进行插值处理，因为往往源图像和目标图像的图像并非一一对应。
* 我们可以用以下公式来表示对每一个像素点（x,y）的重映射：

g(x,y)=f(h(x,y))

where g() is the remapped image, f() the source image and h(x,y) is the mapping function that operates on (x,y).

* Let's think in a quick example. Imagine that we have an image I and, say, we want to do a remap such that:

h(x,y)=(I.cols-x,y)

What would happen? It is easily seen that the image would flip in the x direction. For instance, consider the input image:

![](https://docs.opencv.org/4.1.0/Remap_Tutorial_Theory_0.jpg)

observe how the red circle changes positions with respect to x (considering x the horizontal direction):

![](https://docs.opencv.org/4.1.0/Remap_Tutorial_Theory_1.jpg)

In OpenCV, the function cv::remap offers a simple remapping implementation.