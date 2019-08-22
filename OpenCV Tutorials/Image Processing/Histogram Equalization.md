目的

在这篇教程中，你将会学到：

* 什么是图片的直方图？为什么他很有用？
* 使用OpenCV的cv::equalizeHist函数来显示图像的直方图。

理论

什么是图片的直方图？

* 它代表的是图像强度分布的几何展示。
* 他对图像中的各个考虑的像素强度值进行数量化处理。

![](https://docs.opencv.org/4.1.0/Histogram_Equalization_Theory_0.jpg)

什么是直方方程？

* 他是改善图片对比度的一个方法，是为了拓宽图像的强度值范围（你可以查看维基百科的对应词条）。
* 为了更进一步说明情况，在上面那张图片中，你可以看到似乎在强度值的中间附近有许多像素点分布。直方方程的作用便是将这一部分进行展开。你可以查看以下数据：绿色代表的是不广泛的强度值。使用方程以后，我们得到了中间的图像。而最终图像在下图的右边。

![](https://docs.opencv.org/4.1.0/Histogram_Equalization_Theory_1.jpg)

它是怎么实现的？

* Equalization implies mapping one distribution (the given histogram) to another distribution (a wider and more uniform distribution of intensity values) so the intensity values are spread over the whole range.
* To accomplish the equalization effect, the remapping should be the cumulative distribution function (cdf) (more details, refer to Learning OpenCV). For the histogram H(i), its cumulative distribution H′(i) is:

![](http://latex.codecogs.com/gif.latex?H%5E%7B%27%7D%28i%29%20%3D%20%5Csum_%7B0%20%5Cle%20j%20%3C%20i%7D%20H%28j%29)

To use this as a remapping function, we have to normalize H′(i) such that the maximum value is 255 ( or the maximum value for the intensity of the image ). From the example above, the cumulative function is:

![](https://docs.opencv.org/4.1.0/Histogram_Equalization_Theory_2.jpg)

* Finally, we use a simple remapping procedure to obtain the intensity values of the equalized image:

![](http://latex.codecogs.com/gif.latex?equalized%28%20x%2C%20y%20%29%20%3D%20H%5E%7B%27%7D%28%20src%28x%2Cy%29%20%29)