目的

在本教程中，你将会学到：

* 使用OpenCV的cv::inRange函数进行简单的阈值操作。
* 在HSV色域中检测图像中的元素。

理论

在之前的教程中，我们学习了如何使用cv::threshold函数进行阈值处理。

* 在本教程中，我们将学习cv::inRange函数。
* 概念是差不多的，但是我们有一系列的像素需要处理。

HSV colorspace

HSV (hue, saturation, value) colorspace is a model to represent the colorspace similar to the RGB color model. Since the hue channel models the color type, it is very useful in image processing tasks that need to segment objects based on its color. Variation of the saturation goes from unsaturated to represent shades of gray and fully saturated (no white component). Value channel describes the brightness or the intensity of the color. Next image shows the HSV cylinder.

![](https://docs.opencv.org/4.1.0/Threshold_inRange_HSV_colorspace.jpg)

By SharkDderivative work: SharkD [CC BY-SA 3.0 or GFDL], via Wikimedia Commons
Since colors in the RGB colorspace are coded using the three channels, it is more difficult to segment an object in the image based on its color.

![](https://docs.opencv.org/4.1.0/Threshold_inRange_RGB_colorspace.jpg)

By SharkD [GFDL or CC BY-SA 4.0], from Wikimedia Commons

Formulas used to convert from one colorspace to another colorspace using cv::cvtColor function are described in Color conversions