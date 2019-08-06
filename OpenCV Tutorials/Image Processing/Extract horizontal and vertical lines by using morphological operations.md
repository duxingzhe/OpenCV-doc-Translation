Goal

In this tutorial you will learn how to:

* Apply two very common morphology operators (i.e. Dilation and Erosion), with the creation of custom kernels, in order to extract straight lines on the horizontal and vertical axes. For this purpose, you will use the following OpenCV functions:

    * erode()
    * dilate()
    * getStructuringElement()

in an example where your goal will be to extract the music notes from a music sheet.

Theory

Morphology Operations

Morphology is a set of image processing operations that process images based on predefined structuring elements known also as kernels. The value of each pixel in the output image is based on a comparison of the corresponding pixel in the input image with its neighbors. By choosing the size and shape of the kernel, you can construct a morphological operation that is sensitive to specific shapes regarding the input image.

Two of the most basic morphological operations are dilation and erosion. Dilation adds pixels to the boundaries of the object in an image, while erosion does exactly the opposite. The amount of pixels added or removed, respectively depends on the size and shape of the structuring element used to process the image. In general the rules followed from these two operations have as follows:

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