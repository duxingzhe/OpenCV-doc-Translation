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

* 核函数表达式如下所示:

![](http://latex.codecogs.com/gif.latex?K%3D%5Cdfrac%7B1%7D%7BK_%7Bwidth%7D%5Ccdot%7BK_%7Bheight%7D%7D%7D%5Cbegin%7Bbmatrix%7D1%261%261%26...%261%5C%5C1%261%261%26...%261%5C%5C.%26.%26.%26...%26%201%5C%5C.%26.%26.%26...%261%5C%5C1%261%261%26...%261%5Cend%7Bbmatrix%7D)

高斯滤镜

* 高斯滤镜可能是最有用的滤镜（同样不是最快的滤镜）。高斯滤镜将输入的所有点都进行卷积计算，通过高斯核函数处理并加上他们所有的值来输出最终结果。

* 就让我们简单地说明一下，还记得一维高斯滤镜的变换结果吗？

![](file:///F:/Project/C/OpenCV4.1.0doc/Smoothing_Tutorial_theory_gaussian_0.jpg)

假设图片是一维的，你可以观察到位于中间的像素值会有最大的权重，他周围的像素权重会因为空间距离而下降，中间像素的权重是增加的。

> 注意
> 
> 要记住，二维高斯滤镜可以表示为：
> 
> ![](http://latex.codecogs.com/gif.latex?G_{0}(x,y)=Ae^{\dfrac{-(x-\mu_{x})^{2}}{2\sigma^{2}_{x}}+\dfrac{-(y-\mu_{y})^{2}}{2\sigma^{2}_{y}}})
> 
> μ表示的是方差（峰值）and ![](http://latex.codecogs.com/gif.latex?\sigma^{2})代表的是变量（分别是x和y）的方差

中值滤镜

中值滤镜会处理所有信号的元素（这里是图像），将每一个像素都用其邻近像素的中值来代替（以需处理的像素周围画一个正方形）。

二分滤镜

* 目前，我们主要讨论的是对输入图像进行平滑处理。然而有时候我们不仅要滤镜对噪声进行消除，还要处理边缘。为了达到这一目的（在一定程度上），我们可以使用二分滤镜。

* In an analogous way as the Gaussian filter, the bilateral filter also considers the neighboring pixels with weights assigned to each of them. These weights have two components, the first of which is the same weighting used by the Gaussian filter. The second component takes into account the difference in intensity between the neighboring pixels and the evaluated one.

* For a more detailed explanation you can check this link.
