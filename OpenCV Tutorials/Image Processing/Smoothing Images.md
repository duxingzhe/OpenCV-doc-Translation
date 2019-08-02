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
* To perform a smoothing operation we will apply a filter to our image. The most common type of filters are linear, in which an output pixel's value (i.e. ![](http://latex.codecogs.com/gif.latex?g(i,j))) is determined as a weighted sum of input pixel values (i.e. ![](http://latex.codecogs.com/gif.latex?f(i+k,j+l))) :

![](http://latex.codecogs.com/gif.latex?g(i,j)=\sum_{k,l}f(i+k,j+l)h(k,l))

![](http://latex.codecogs.com/gif.latex?h(k,l)) is called the kernel, which is nothing more than the coefficients of the filter.

It helps to visualize a filter as a window of coefficients sliding across the image.

* There are many kind of filters, here we will mention the most used:

Normalized Box Filter

* This filter is the simplest of all! Each output pixel is the mean of its kernel neighbors ( all of them contribute with equal weights)

* The kernel is below:

![](http://latex.codecogs.com/gif.download?K%3D%5Cdfrac%7B1%7D%7BK_%7Bwidth%7D%5Ccdot%7BK_%7Bheight%7D%7D%7D%5Cbegin%7Bbmatrix%7D1%261%261%26...%261%5C%5C1%261%261%26...%261%5C%5C.%26.%26.%26...%26%201%5C%5C.%26.%26.%26...%261%5C%5C1%261%261%26...%261%5Cend%7Bbmatrix%7D)

Gaussian Filter

* Probably the most useful filter (although not the fastest). Gaussian filtering is done by convolving each point in the input array with a Gaussian kernel and then summing them all to produce the output array.

* Just to make the picture clearer, remember how a 1D Gaussian kernel look like?