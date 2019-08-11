目的

在本教程中，你将会学到：

* 使用OpenCV的cv::inRange函数进行简单的阈值操作。
* 在HSV色域中检测图像中的元素。

理论

在之前的教程中，我们学习了如何使用cv::threshold函数进行阈值处理。

* 在本教程中，我们将学习cv::inRange函数。
* 概念是差不多的，但是我们有一系列的像素需要处理。

HSV色域

HSV(色调 hue、饱和 saturation、值 value)颜色空间是类似于RGB的色彩空间模型。如果我们使用了hue通道来建立颜色模型，那么我们就可以基于他们的颜色快速分离这些物体。饱和度的范围从表示阴影的灰度不完全饱和到完全饱和（并不是白色部分）。通道的值表示的是颜色的亮度或者是强度关系。下面的图形表示的便是HSV圆柱图。

![](https://docs.opencv.org/4.1.0/Threshold_inRange_HSV_colorspace.jpg)

By SharkDderivative work: SharkD [CC BY-SA 3.0 or GFDL], via Wikimedia Commons

因为RGB会把颜色分成三个通道，这也就使得基于他们的颜色分离物体的难度大为增加。

![](https://docs.opencv.org/4.1.0/Threshold_inRange_RGB_colorspace.jpg)

By SharkD [GFDL or CC BY-SA 4.0], from Wikimedia Commons

在颜色转换教程中，我们会通过cv::cvtColor来介绍将一种色域转换为另一种色域的公式。