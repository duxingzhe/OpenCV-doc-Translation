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

一个有核的修正操作是怎么样的？

假如你想知道图像中具体位置的操作结果。我们可以通过以下方式计算修正操作的值：

1.将核放置在确定像素的顶端，同时核的其他部分覆盖图像的其他对应区域。
2.用对应图像像素乘以核系数，并加上全部的值。
3.将结果保存在输入图像中锚点对应位置。
4.通过对核进行遍历对整个图像覆盖的像素点进行相同操作。

以上步骤可以用以下公式来表示：

![](http://latex.codecogs.com/gif.latex?H(x,y)=\sum_{i=0}^{M_{i}-1}\sum_{j=0}^{M_{j}-1}I(x+i-a_{i},y+j-a_{j})K(i,j))

幸运的是，OpenCV提供了filter2D()函数，所以你可以少写很多代码。

What does this program do?

* Loads an image
* Performs a normalized box filter. For instance, for a kernel of size size=3, the kernel would be:

![](http://latex.codecogs.com/gif.latex?K%3D%5Cdfrac%7B1%7D%7B3%5Ccdot%7B3%7D%7D%5Cbegin%7Bbmatrix%7D1%261%261%5C%5C1%261%261%5C%5C1%261%261%5Cend%7Bbmatrix%7D)

The program will perform the filter operation with kernels of sizes 3, 5, 7, 9 and 11.

* The filter output (with each kernel) will be shown during 500 milliseconds