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

* 方程其实表示的是对一种分布的映射（我们提供的直方图）到另一种分布（强度值范围更宽更统一的分布），因此强度值可以完全分布于整个范围。
* 为了完善公式的效果，重映射应该进行累积分布函数的计算（cumulative distribution function，cdf）（更多内容请参阅《Learning OpenCV》一书）。对于直方方程H(i)，其累积分布函数为：

![](http://latex.codecogs.com/gif.latex?H%5E%7B%27%7D%28i%29%20%3D%20%5Csum_%7B0%20%5Cle%20j%20%3C%20i%7D%20H%28j%29)

为了将他用成一个重映射函数，我们需要对H′(i)进行归一化处理，以确保最大值为255（或者图像强度值的最大值）。对于之前的例子，累计函数为：

![](https://docs.opencv.org/4.1.0/Histogram_Equalization_Theory_2.jpg)

* 最终，我们对已经通过直方方程处理过后的图像使用了简单的重映射生成函数。

![](http://latex.codecogs.com/gif.latex?equalized%28%20x%2C%20y%20%29%20%3D%20H%5E%7B%27%7D%28%20src%28x%2Cy%29%20%29)