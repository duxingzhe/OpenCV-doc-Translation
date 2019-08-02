目标

在这篇教程里，你讲学习到如何使用OpenCV函数对各种各样的线性变换来对图像进行模糊处理，例如：

* blur()
* GaussianBlur()
* medianBlur()
* bilateralFilter()

理论

> 注意
>
> 本教程的例子选自Richard Szeliski的《机器视觉：算法和应用》一书以及OpenCV入门教程。

* 平滑处理，抑或叫做模糊处理，是一种简单又常见的图像处理操作。
* 我们有许多原因需要进行平滑处理。在这篇教程中，我们将对图片进行平滑处理以便去除噪声（其他的用法将会在接下来的教程中一一讲解）。
* 为了能够执行对图像的平滑处理，我们将对图像加上一个滤镜。最常用的滤镜方式是线性滤镜，也就是说，对于一个输出的像素值(i.e. ![](http://latex.codecogs.com/gif.latex?g(i,j)))取决于输入值的加权处理结果(i.e. ![](http://latex.codecogs.com/gif.latex?f(i+k,j+l))) :

![](http://latex.codecogs.com/gif.latex?g(i,j)=\sum_{k,l}f(i+k,j+l)h(k,l))

![](http://latex.codecogs.com/gif.latex?h(k,l))被称作核，就是滤镜中的一个系数。

它可以将图像中的平滑系数进行可视化显示。

* 有很多可以使用的滤镜，这里我们将介绍一些特别常用的：

归一化块滤波

* 这是一个最简单的滤镜！所有的输出像素值是以其为中心加和平均值（所有像素的权重是一致的）

* The kernel is below:

![](http://latex.codecogs.com/gif.latex?K%3D%5Cdfrac%7B1%7D%7BK_%7Bwidth%7D%5Ccdot%7BK_%7Bheight%7D%7D%7D%5Cbegin%7Bbmatrix%7D1%261%261%26...%261%5C%5C1%261%261%26...%261%5C%5C.%26.%26.%26...%26%201%5C%5C.%26.%26.%26...%261%5C%5C1%261%261%26...%261%5Cend%7Bbmatrix%7D)

Gaussian Filter

* Probably the most useful filter (although not the fastest). Gaussian filtering is done by convolving each point in the input array with a Gaussian kernel and then summing them all to produce the output array.

* Just to make the picture clearer, remember how a 1D Gaussian kernel look like?
