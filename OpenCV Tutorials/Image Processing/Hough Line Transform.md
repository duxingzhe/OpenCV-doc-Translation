目标

在本篇教程中，你将会学到：

* 使用OpenCV的HoughLines()和HoughLinesP()来检测图像的线条。

理论介绍

> 注意
>
> 这些例子来自Bradski和Kaehler所著的Learning OpenCV一书。

霍夫线条变换

1.霍夫线条变换是用于检测直线的。

2.为了能使用这个变换，首先，我们应该先执行边界检测。

他是怎么做的？

1.正如你知道的，图像空间中，直线可以用两个变量来表示。例如：

    a.直角坐标系：变量为：(m,b)。
    b.极坐标系：变量为：(r,θ)。

![](https://docs.opencv.org/4.1.0/Hough_Lines_Tutorial_Theory_0.jpg)

在霍夫变换中，我们将用极坐标方式处理。因此，直线的方程可以用如下公式表示：

![](http://latex.codecogs.com/gif.latex?y%20%3D%20%5Cleft%20%28%20-%5Cdfrac%7B%5Ccos%20%5Ctheta%7D%7B%5Csin%20%5Ctheta%7D%20%5Cright%20%29%20x%20+%20%5Cleft%20%28%20%5Cdfrac%7Br%7D%7B%5Csin%20%5Ctheta%7D%20%5Cright%20%29)

我们有如下公式：r=xcosθ+ysinθ

1.通常而言，对于每一个点，![](http://latex.codecogs.com/gif.latex?%28x_%7B0%7D%2C%20y_%7B0%7D%29)，我们可以定义 这一定点的直线为：

![](http://latex.codecogs.com/gif.latex?r_%7B%5Ctheta%7D%20%3D%20x_%7B0%7D%20%5Ccdot%20%5Ccos%20%5Ctheta%20+%20y_%7B0%7D%20%5Ccdot%20%5Csin%20%5Ctheta)

意味着每一对坐标![](http://latex.codecogs.com/gif.latex?%28r_%7B%5Ctheta%7D%2C%5Ctheta%29)代表了通过![](http://latex.codecogs.com/gif.latex?%28x_%7B0%7D%2C%20y_%7B0%7D%29)的每一条直线。

2.给定点![](http://latex.codecogs.com/gif.latex?%28x_%7B0%7D%2C%20y_%7B0%7D%29)，我们将经过这个地方的直线全部画出来，我们会得到正弦波。例如x0=8 and y0=6，我们会得到一下图形(在极坐标θ-r情况下)：

![](https://docs.opencv.org/4.1.0/Hough_Lines_Tutorial_Theory_1.jpg)

我们只考虑r>0且0<θ<2π的情况。

3.在图像里，我们可以对所有的点进行同样的操作。如果经过两个点的曲线在极坐标θ-r情况下有相交的情况，意味着他们是在同一条直线上。例如，下面的例子中，前者表示的是多个点之间连线：![](http://latex.codecogs.com/gif.latex?x_%7B1%7D%20%3D%204)，![](http://latex.codecogs.com/gif.latex?y_%7B1%7D%20%3D%209)和![](http://latex.codecogs.com/gif.latex?x_%7B2%7D%20%3D%2012)，![](http://latex.codecogs.com/gif.latex?y_%7B2%7D%20%3D%203)，我们得到了：

![](https://docs.opencv.org/4.1.0/Hough_Lines_Tutorial_Theory_2.jpg)

这三条线交于一个点(0.925, 9.6)，这三条线使用极坐标变量(θ,r)来表示线便是：![](http://latex.codecogs.com/gif.latex?%28x_%7B0%7D%2C%20y_%7B0%7D%29)，![](http://latex.codecogs.com/gif.latex?%28x_%7B1%7D%2C%20y_%7B1%7D%29)和![](http://latex.codecogs.com/gif.latex?%28x_%7B2%7D%2C%20y_%7B2%7D%29).

4.上面的这些信息说明了什么？他说明，一般而言，直线可以被曲线相交的交点个数来检测出来。更多的曲线交点意味着直线可以用更多的交点所表示。通常而言，我们需要一个最低阈值来确定表示直线的交点个数。

5.这就是霍夫直线变换所做的内容。他对图像中的各点曲线交点持续追踪。如果交点的个数大于阈值，那么他就表示了一条直线，直线是用交点坐标![](http://latex.codecogs.com/gif.latex?%28%5Ctheta%2C%20r_%7B%5Ctheta%7D%29)表示的。

标准霍夫直线变换和概率霍夫直线变换

OpenCV实现了两种霍夫直线变换：

a.标准霍夫变换

* 它包括了许多我们之前谈到的变换。它可以产生一对对变量集合![](http://latex.codecogs.com/gif.latex?%28%5Ctheta%2C%20r_%7B%5Ctheta%7D%29)
* 在OpenCV里，我们可以使用HoughLines()来实现这种变换。

b.概率型霍夫变换

* 这是霍夫直线变换中更有效率的一种实现。他可以输出检测直线的极值![](http://latex.codecogs.com/gif.latex?%28x_%7B0%7D%2C%20y_%7B0%7D%2C%20x_%7B1%7D%2C%20y_%7B1%7D%29)
* 在OpenCV里，我们可以使用HoughLinesP()来实现这种变换。

What does this program do?

* Loads an image
* Applies a Standard Hough Line Transform and a Probabilistic Line Transform.
* Display the original image and the detected line in three windows.