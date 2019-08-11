目的

在这篇教程中，你将会学到：

* 使用OpenCV的filter2D()函数实现你自己的线性滤镜。

理论

> 注意
> 
> 本文例子来源于Bradski和Kaehler所著的Learning OpenCV一书。

修正

在一个非常通用的场景中，修正是在图像的每一部分和操作（核）之间的处理过程。

什么是核？

核通常是一个固定大小的数组元素，里面包含了数值系数和一个锚点，一般而言锚点会位于数组的中心位置。

![](https://docs.opencv.org/4.1.0/filter_2d_tutorial_kernel_theory.png)

How does correlation with a kernel work?

Assume you want to know the resulting value of a particular location in the image. The value of the correlation is calculated in the following way:

1.Place the kernel anchor on top of a determined pixel, with the rest of the kernel overlaying the corresponding local pixels in the image.
2.Multiply the kernel coefficients by the corresponding image pixel values and sum the result.
3.Place the result to the location of the anchor in the input image.
4.Repeat the process for all pixels by scanning the kernel over the entire image.

Expressing the procedure above in the form of an equation we would have:

![](http://latex.codecogs.com/gif.latex?H(x,y)=\sum_{i=0}^{M_{i}-1}\sum_{j=0}^{M_{j}-1}I(x+i-a_{i},y+j-a_{j})K(i,j))

Fortunately, OpenCV provides you with the function filter2D() so you do not have to code all these operations.

What does this program do?

* Loads an image
* Performs a normalized box filter. For instance, for a kernel of size size=3, the kernel would be:

![](http://latex.codecogs.com/gif.latex?K%3D%5Cdfrac%7B1%7D%7B3%5Ccdot%7B3%7D%7D%5Cbegin%7Bbmatrix%7D1%261%261%5C%5C1%261%261%5C%5C1%261%261%5Cend%7Bbmatrix%7D)

The program will perform the filter operation with kernels of sizes 3, 5, 7, 9 and 11.

* The filter output (with each kernel) will be shown during 500 milliseconds