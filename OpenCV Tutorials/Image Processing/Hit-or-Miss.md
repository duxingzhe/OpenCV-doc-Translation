目标


在这篇教程中，你将学到如何使用击中与否变换方法在一张二值图中找到给定配置或模型。这种变换也是其他高级形态学操作的基础，例如稀释或修剪。

我们将使用OpenCV的morphologyEx()函数。

击中与否变换原理

形态学对图像的操作主要是基于图像中物体的形状。这些操作在图形中一个或多个结构体上进行操作。两种基本的形态学操作室侵蚀和扩张。这两种基本操作可以产生其他更为高级的形态学转化，比如打开、闭合或顶帽变换。为了能了解这些和其他的基本形态学变换操作，你可以查阅之前的教程（侵蚀和扩张以及更多形态学变换）。

击中与否变换对于寻找二值图中的模型有非常好的效果。他特别对于其领域匹配的上那些第一个结构体B1的元素，却匹配不上邻近元素B2的形状。数学上，这项操作对于图像A的表达式是：

![](http://latex.codecogs.com/gif.latex?A\circledast{B}=(A\ominus{B_1})\cap(A^c\ominus{B_2}))

因此，击中与否变换包括以下三个步骤：

1.用B1侵蚀图像A。

2.用元素B2侵蚀图像A的剩下部分。

3.将步骤一和步骤二的图形做与运算。

元素B1和B2可以合成一个简单的元素B。让我们来看个例子：

![](https://docs.opencv.org/4.1.0/hitmiss_kernels.png)

Structuring elements (kernels). Left: kernel to 'hit'. Middle: kernel to 'miss'. Right: final combined kernel

在这个例子当中，我们需要寻找中心像素是属于背景的，而东西南北的元素是属于前景的。而邻近元素则是任何一种，我们并不关注。现在我们可以用以下核函数对输入图像进行处理：

![](https://docs.opencv.org/4.1.0/hitmiss_input.png)

输入二值图

![](https://docs.opencv.org/4.1.0/hitmiss_output.png)

输出二值图

你可以看到在这张图片的某一地方应用了一个模型。