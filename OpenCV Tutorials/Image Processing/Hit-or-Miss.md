目标


在这篇教程中，你将学到如何使用击中与否变换方法在一张二值图中找到给定配置或模型。这种变换也是其他高级形态学操作的基础，例如稀释或修剪。

我们将使用OpenCV的morphologyEx()函数。

击中与否变换原理

形态学对图像的操作主要是基于图像中物体的形状。这些操作在图形中一个或多个结构体上进行操作。两种基本的形态学操作室侵蚀和扩张。这两种基本操作可以产生其他更为高级的形态学转化，比如打开、闭合或顶帽变换。为了能了解这些和其他的基本形态学变换操作，你可以查阅之前的教程（侵蚀和扩张以及更多形态学变换）。

击中与否变换对于寻找二值图中的模型有非常好的效果。他特别对于其领域匹配的上那些第一个结构体B1的元素，却匹配不上邻近元素B2的形状。数学上，这项操作对于图像A的表达式是：

![](http://latex.codecogs.com/gif.latex?A\circledast{B}=(A\ominus{B_1})\cap(A^c\ominus{B_2}))

Therefore, the hit-or-miss operation comprises three steps:

Erode image A with structuring element B1.

Erode the complement of image A ( Ac) with structuring element B2.

AND results from step 1 and step 2.

The structuring elements B1 and B2 can be combined into a single element B. Let's see an example:

![](https://docs.opencv.org/4.1.0/hitmiss_kernels.png)

Structuring elements (kernels). Left: kernel to 'hit'. Middle: kernel to 'miss'. Right: final combined kernel

In this case, we are looking for a pattern in which the central pixel belongs to the background while the north, south, east, and west pixels belong to the foreground. The rest of pixels in the neighbourhood can be of any kind, we don't care about them. Now, let's apply this kernel to an input image:

![](https://docs.opencv.org/4.1.0/hitmiss_input.png)

Input binary image

![](https://docs.opencv.org/4.1.0/hitmiss_output.png)

Output binary image

You can see that the pattern is found in just one location within the image.