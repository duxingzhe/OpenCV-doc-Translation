目标

在这篇文章中，你将会学到：

使用OpenCV中的Laplacian()函数用拉普拉斯算子进行离散相似物操作。

理论

在之前的教程中我们知道如何使用索贝尔算子。这是基于以下的原理：在边界区域，像素的强度值会有一个巨大的“跳跃性”改变或者是强度值的巨大变化。强度的第一个派生值给出以后，我们发现边界是根据最大值给定的，也就是说，在下面的图片所示：

![](https://docs.opencv.org/4.1.0/Laplace_Operator_Tutorial_Theory_Previous.jpg)

并且……如果我们分析了第二个派生值？

![](https://docs.opencv.org/4.1.0/Laplace_Operator_Tutorial_Theory_ddIntensity.jpg)

你可以看到第二个派生值为0！因此我们可以看到这个值也可以成为检测边界的方法。但是，注意到0值可能不只是出现在边界上（他们还可以出现在其他无意义的地方上）；因此如果我们想使用第二个派生值的话，我们可一个使用我们想使用的滤镜来处理图片。

拉普拉斯算子

From the explanation above, we deduce that the second derivative can be used to detect edges. Since images are "\*2D\*", we would need to take the derivative in both dimensions. Here, the Laplacian operator comes handy.

The Laplacian operator is defined by:

![](http://latex.codecogs.com/gif.latex?Laplace(f)=\dfrac{\partial^{2}f}{\partialx^{2}}+\dfrac{\partial^{2}f}{\partial{y}^{2}})

The Laplacian operator is implemented in OpenCV by the function Laplacian() . In fact, since the Laplacian uses the gradient of images, it calls internally the Sobel operator to perform its computation.