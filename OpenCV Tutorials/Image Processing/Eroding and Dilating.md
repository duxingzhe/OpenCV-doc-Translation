目的

在这篇教程中你将会学到：

* 应用两种非常常用的形态学操作：腐蚀和扩张。为了完成这项操作，你可以使用一下OpenCV函数：
    * cv::erode
    * cv::dilate

> 注意
> 
> 本教程的例子选自Richard Szeliski的《机器视觉：算法和应用》一书

形态学操作

* In short: A set of operations that process images based on shapes. Morphological operations apply a structuring element to an input image and generate an output image.

* The most basic morphological operations are: Erosion and Dilation. They have a wide array of uses, i.e. :

    * Removing noise
    * Isolation of individual elements and joining disparate elements in an image.
    * Finding of intensity bumps or holes in an image

* We will explain dilation and erosion briefly, using the following image as an example:

![](https://docs.opencv.org/4.1.0/Morphology_1_Tutorial_Theory_Original_Image.png)

Dilation

* This operations consists of convolving an image A with some kernel ( B), which can have any shape or size, usually a square or circle.
* The kernel B has a defined anchor point, usually being the center of the kernel.
* As the kernel B is scanned over the image, we compute the maximal pixel value overlapped by B and replace the image pixel in the anchor point position with that maximal value. As you can deduce, this maximizing operation causes bright regions within an image to "grow" (therefore the name dilation).
* The dilatation operation is:![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=\max_{(x',y'):\,\texttt{element}(x',y')\ne0}\texttt{src}(x+x',y+y'))
* Take the above image as an example. Applying dilation we can get:

![](https://docs.opencv.org/4.1.0/Morphology_1_Tutorial_Theory_Dilation.png)

* The bright area of the letter dilates around the black regions of the background.

Erosion

* This operation is the sister of dilation. It computes a local minimum over the area of given kernel.
* As the kernel B is scanned over the image, we compute the minimal pixel value overlapped by B and replace the image pixel under the anchor point with that minimal value.
The erosion operation is: ![](http://latex.codecogs.com/gif.latex?\texttt{dst}(x,y)=\min_{(x',y'):\,\texttt{element}(x',y')\ne0}\texttt{src}(x+x',y+y'))

* Analagously to the example for dilation, we can apply the erosion operator to the original image (shown above). You can see in the result below that the bright areas of the image get thinner, whereas the dark zones gets bigger.

![](https://docs.opencv.org/4.1.0/Morphology_1_Tutorial_Theory_Erosion.png)