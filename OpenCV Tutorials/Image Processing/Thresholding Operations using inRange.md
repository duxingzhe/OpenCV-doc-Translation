Goal

In this tutorial you will learn how to:

* Perform basic thresholding operations using OpenCV cv::inRange function.
* Detect an object based on the range of pixel values in the HSV colorspace.

Theory

In the previous tutorial, we learnt how to perform thresholding using cv::threshold function.

* In this tutorial, we will learn how to do it using cv::inRange function.
* The concept remains the same, but now we add a range of pixel values we need.

HSV colorspace

HSV (hue, saturation, value) colorspace is a model to represent the colorspace similar to the RGB color model. Since the hue channel models the color type, it is very useful in image processing tasks that need to segment objects based on its color. Variation of the saturation goes from unsaturated to represent shades of gray and fully saturated (no white component). Value channel describes the brightness or the intensity of the color. Next image shows the HSV cylinder.

![](https://docs.opencv.org/4.1.0/Threshold_inRange_HSV_colorspace.jpg)

By SharkDderivative work: SharkD [CC BY-SA 3.0 or GFDL], via Wikimedia Commons
Since colors in the RGB colorspace are coded using the three channels, it is more difficult to segment an object in the image based on its color.

![](https://docs.opencv.org/4.1.0/Threshold_inRange_RGB_colorspace.jpg)

By SharkD [GFDL or CC BY-SA 4.0], from Wikimedia Commons

Formulas used to convert from one colorspace to another colorspace using cv::cvtColor function are described in Color conversions