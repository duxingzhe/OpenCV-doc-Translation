目的

从这篇教程中，你将会学习到：

* 使用两种非常常用的形态学操作（如侵蚀和扩张），使用自定义的核，这样就能直接提取到垂直和水平方向的直线。为了能够达到这一目的，你需要以下OpenCV函数：

    * erode()
    * dilate()
    * getStructuringElement()

接下来的例子中，你将会知道如何从乐谱中提取出音符。

理论

形态学操作

形态学是一系列基于预定义元素（同时也被称作核）的图像处理技术。每一个输出图像像素的值都基于输入函数对应像素的临近像素之间的比较。当你选择了核的大小和形状以后，你就可以对输入图像创建一种基于特定形状的形态学操作。

两种最基本的形态学操作是侵蚀和扩张。侵蚀会将图像中目标对象的像素加载物体的边缘上，而扩张则正好相反。增加或减少像素的数量基于对图像处理中使用的预定义元素大小和形状。通常情况下，这两个操作的规则是：

* Dilation: The value of the output pixel is the maximum value of all the pixels that fall within the structuring element's size and shape. For example in a binary image, if any of the pixels of the input image falling within the range of the kernel is set to the value 1, the corresponding pixel of the output image will be set to 1 as well. The latter applies to any type of image (e.g. grayscale, bgr, etc).

![](https://docs.opencv.org/4.1.0/morph21.gif)

Dilation on a Binary Image

![](https://docs.opencv.org/4.1.0/morph6.gif)

Dilation on a Grayscale Image

* Erosion: The vise versa applies for the erosion operation. The value of the output pixel is the minimum value of all the pixels that fall within the structuring element's size and shape. Look the at the example figures below:

![](https://docs.opencv.org/4.1.0/morph211.png)

Erosion on a Binary Image

![](https://docs.opencv.org/4.1.0/morph61.png)

Erosion on a Grayscale Image

Structuring Elements

As it can be seen above and in general in any morphological operation the structuring element used to probe the input image, is the most important part.

A structuring element is a matrix consisting of only 0's and 1's that can have any arbitrary shape and size. Typically are much smaller than the image being processed, while the pixels with values of 1 define the neighborhood. The center pixel of the structuring element, called the origin, identifies the pixel of interest – the pixel being processed.

For example, the following illustrates a diamond-shaped structuring element of 7x7 size.

![](https://docs.opencv.org/4.1.0/morph12.gif)

A Diamond-Shaped Structuring Element and its Origin

A structuring element can have many common shapes, such as lines, diamonds, disks, periodic lines, and circles and sizes. You typically choose a structuring element the same size and shape as the objects you want to process/extract in the input image. For example, to find lines in an image, create a linear structuring element as you will see later.