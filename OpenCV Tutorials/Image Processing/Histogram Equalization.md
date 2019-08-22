目的

在这篇教程中，你将会学到：

* 什么是图片的直方图？为什么他很有用？
* 使用OpenCV的cv::equalizeHist函数来显示图像的直方图。

理论

什么是图片的直方图？

* 它代表的是图像强度分布的几何展示。
* 他对图像中的各个考虑的像素强度值进行数量化处理。

![](https://docs.opencv.org/4.1.0/Histogram_Equalization_Theory_0.jpg)

What is Histogram Equalization?

* It is a method that improves the contrast in an image, in order to stretch out the intensity range (see also the corresponding Wikipedia entry).
* To make it clearer, from the image above, you can see that the pixels seem clustered around the middle of the available range of intensities. What Histogram Equalization does is to stretch out this range. Take a look at the figure below: The green circles indicate the underpopulated intensities. After applying the equalization, we get an histogram like the figure in the center. The resulting image is shown in the picture at right.

![](https://docs.opencv.org/4.1.0/Histogram_Equalization_Theory_1.jpg)

How does it work?

* Equalization implies mapping one distribution (the given histogram) to another distribution (a wider and more uniform distribution of intensity values) so the intensity values are spread over the whole range.
* To accomplish the equalization effect, the remapping should be the cumulative distribution function (cdf) (more details, refer to Learning OpenCV). For the histogram H(i), its cumulative distribution H′(i) is:

![](http://latex.codecogs.com/gif.latex?H%5E%7B%27%7D%28i%29%20%3D%20%5Csum_%7B0%20%5Cle%20j%20%3C%20i%7D%20H%28j%29)

To use this as a remapping function, we have to normalize H′(i) such that the maximum value is 255 ( or the maximum value for the intensity of the image ). From the example above, the cumulative function is:

![](https://docs.opencv.org/4.1.0/Histogram_Equalization_Theory_2.jpg)

* Finally, we use a simple remapping procedure to obtain the intensity values of the equalized image:

![](http://latex.codecogs.com/gif.latex?equalized%28%20x%2C%20y%20%29%20%3D%20H%5E%7B%27%7D%28%20src%28x%2Cy%29%20%29)