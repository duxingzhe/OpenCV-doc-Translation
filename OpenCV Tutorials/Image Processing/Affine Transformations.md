目的

在这篇教程中，你将会学到：

* 使用OpenCV的cv::warpAffine函数来实现简单的重映射路径。
* 使用OpenCV的cv::getRotationMatrix2D实现一个2x3旋转矩阵。

理论

什么是仿射变换？

1.这种变换可以用矩阵乘法的形式表达（线性变换），这个算法是在加了向量以后实现的（平移）。
2.综上所述，我们可以把仿射变换归为以下几类：

    a.旋转（线性变换）
    b.平移（向量加法）
    c.按比例扩大（线性变换）

由此，你也看出来了，实际上，放射变换表示了两张图片之间的关系。

3.通常情况下，我们会用一个2x3矩阵来表示一个仿射变换。

![](http://latex.codecogs.com/gif.latex?A%20%3D%20%5Cbegin%7Bbmatrix%7D%20a_%7B00%7D%20%26%20a_%7B01%7D%20%5C%5C%20a_%7B10%7D%20%26%20a_%7B11%7D%20%5Cend%7Bbmatrix%7D_%7B2%20%5Ctimes%202%7D%20B%20%3D%20%5Cbegin%7Bbmatrix%7D%20b_%7B00%7D%20%5C%5C%20b_%7B10%7D%20%5Cend%7Bbmatrix%7D_%7B2%20%5Ctimes%201%7D)

![](http://latex.codecogs.com/gif.latex?M%20%3D%20%5Cbegin%7Bbmatrix%7D%20A%20%26%20B%20%5Cend%7Bbmatrix%7D%20%3D%20%5Cbegin%7Bbmatrix%7D%20a_%7B00%7D%20%26%20a_%7B01%7D%20%26%20b_%7B00%7D%20%5C%5C%20a_%7B10%7D%20%26%20a_%7B11%7D%20%26%20b_%7B10%7D%20%5Cend%7Bbmatrix%7D_%7B2%20%5Ctimes%203%7D)

考虑一下，我们想用A和B来转换一个2D向量![](http://latex.codecogs.com/gif.download?X%20%3D%20%5Cbegin%7Bbmatrix%7Dx%20%5C%5C%20y%5Cend%7Bbmatrix%7D)， 我们可以用以下公式来做同样的事情：

![](http://latex.codecogs.com/gif.latex?T%20%3D%20A%20%5Ccdot%20%5Cbegin%7Bbmatrix%7Dx%20%5C%5C%20y%5Cend%7Bbmatrix%7D%20+%20B)

或

![](http://latex.codecogs.com/gif.latex?T%20%3D%20M%20%5Ccdot%20%5Bx%2C%20y%2C%201%5D%5E%7BT%7D)

![](http://latex.codecogs.com/gif.latex?T%20%3D%20%5Cbegin%7Bbmatrix%7D%20a_%7B00%7Dx%20+%20a_%7B01%7Dy%20+%20b_%7B00%7D%20%5C%5C%20a_%7B10%7Dx%20+%20a_%7B11%7Dy%20+%20b_%7B10%7D%20%5Cend%7Bbmatrix%7D)

我们如何得到一个仿射变换？

1.我们提到了，一个仿射变换通常把两张图片之间建立了联系。简单来说，这个关系可以用两个方法来体现：

    a.我们都知道X和T，我们也直到他们是有关系的。所以，我们的任务是找到M；
    b.我们知道M和X。获得T，我们只需要实现T=M*X。我们对于M的信息可以更为直观（例如有一个2x3矩阵）或者是点与点之间的几何关系。

2.让我们用更好的方式来解释（b）。如果M建立了两张图片的关系，我们就能分析这两张图片中三个点的关系。看看下面的图片：

![](https://docs.opencv.org/4.1.0/Warp_Affine_Tutorial_Theory_0.jpg)

上面的1、2、3（在图像一中组成了一个三角形）向图像二映射，这样也组成了一个三角形。但是这两个三角形的形状有了巨大的改变。如果我们能找到这三个点的关系（你可以用你的喜欢的关系），我们就能用这个关系来找到一个图片里所有像素的关系。