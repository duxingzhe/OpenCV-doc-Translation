目的

在这篇教程中你将学到：

* 什么叫特征，为什么它很重要
* 使用cv::cornerHarris函数来用哈里斯——史蒂芬斯方法来检测边角

理论

什么是特征？

* 在机器视觉中，通常我们需要在一个环境中不同的框架中找到匹配的点。为什么？因为，如果我们知道了两张图片之间是如何联系的，我们就能扩展这两张图片的信息。
* 当我们说我们在匹配点的时候，通常情况下，当我们能够容易发现的时候我们会说这张图具有特点。我们会说这张图片有明显的特点。
* 所以，图片的特点需要有什么特点呢？
    
    * 特点必须非常独特

图像特点的类型

本文主要涉及以下类型：

* 边
* 角（同样被称之为兴趣点）
* 连通区域（同样被称之为感兴趣区域）

在这篇博文中，我们重点关注角的特性。

为什么角会如此特别？

* 因为他是两条边的交点，它代表着两条边因此发生了方向改变。因此，图像的径向（在各个方向上）有了巨大的变化，由此就可以被检测到。

他是如何工作的？

* 我们来看看边角。边角代表的是图片中径向的变化，我们就会去寻找这个“变量”。
* 先考虑一张灰阶图I。我们将会将他平铺到窗口上w(x,y)（通过覆盖x方向的u和y方向的v）I，并计算强度的变化值。

![](http://latex.codecogs.com/gif.latex?E%28u%2Cv%29%20%3D%20%5Csum%20_%7Bx%2Cy%7D%20w%28x%2Cy%29%5B%20I%28x+u%2Cy+v%29%20-%20I%28x%2Cy%29%5D%5E%7B2%7D)

其中:

    * w(x,y) 窗体的位置 (x,y)
    * I(x,y) (x,y)的强度值
    * I(x+u,y+v) 是移动窗口w(x+u,y+v)的强度

* Since we are looking for windows with corners, we are looking for windows with a large variation in intensity. Hence, we have to maximize the equation above, specifically the term:

![](http://latex.codecogs.com/gif.latex?%5Csum%20_%7Bx%2Cy%7D%5B%20I%28x+u%2Cy+v%29%20-%20I%28x%2Cy%29%5D%5E%7B2%7D)

* Using Taylor expansion:

![](http://latex.codecogs.com/gif.latex?E%28u%2Cv%29%20%5Capprox%20%5Csum%20_%7Bx%2Cy%7D%5B%20I%28x%2Cy%29%20+%20u%20I_%7Bx%7D%20+%20vI_%7By%7D%20-%20I%28x%2Cy%29%5D%5E%7B2%7D)

* Expanding the equation and cancelling properly:

![](http://latex.codecogs.com/gif.latex?E%28u%2Cv%29%20%5Capprox%20%5Csum%20_%7Bx%2Cy%7D%20u%5E%7B2%7DI_%7Bx%7D%5E%7B2%7D%20+%202uvI_%7Bx%7DI_%7By%7D%20+%20v%5E%7B2%7DI_%7By%7D%5E%7B2%7D)

* Which can be expressed in a matrix form as:

![](http://latex.codecogs.com/gif.latex?E%28u%2Cv%29%20%5Capprox%20%5Cbegin%7Bbmatrix%7D%20u%20%26%20v%20%5Cend%7Bbmatrix%7D%20%5Cleft%20%28%20%5Cdisplaystyle%20%5Csum_%7Bx%2Cy%7D%20w%28x%2Cy%29%20%5Cbegin%7Bbmatrix%7D%20I_x%5E%7B2%7D%20%26%20I_%7Bx%7DI_%7By%7D%20%5C%5C%20I_xI_%7By%7D%20%26%20I_%7By%7D%5E%7B2%7D%20%5Cend%7Bbmatrix%7D%20%5Cright%20%29%20%5Cbegin%7Bbmatrix%7D%20u%20%5C%5C%20v%20%5Cend%7Bbmatrix%7D)

* Let's denote:

![](http://latex.codecogs.com/gif.latex?M%20%3D%20%5Cdisplaystyle%20%5Csum_%7Bx%2Cy%7D%20w%28x%2Cy%29%20%5Cbegin%7Bbmatrix%7D%20I_x%5E%7B2%7D%20%26%20I_%7Bx%7DI_%7By%7D%20%5C%5C%20I_xI_%7By%7D%20%26%20I_%7By%7D%5E%7B2%7D%20%5Cend%7Bbmatrix%7D)

* So, our equation now is:

![](http://latex.codecogs.com/gif.download?E%28u%2Cv%29%20%5Capprox%20%5Cbegin%7Bbmatrix%7D%20u%20%26%20v%20%5Cend%7Bbmatrix%7D%20M%20%5Cbegin%7Bbmatrix%7D%20u%20%5C%5C%20v%20%5Cend%7Bbmatrix%7D)

A score is calculated for each window, to determine if it can possibly contain a corner:

![](http://latex.codecogs.com/gif.download?R%20%3D%20det%28M%29%20-%20k%28trace%28M%29%29%5E%7B2%7D)

    where:

    * det(M) = ![](http://latex.codecogs.com/gif.download?%5Clambda_%7B1%7D%5Clambda_%7B2%7D)
    * trace(M) = ![](http://latex.codecogs.com/gif.download?%5Clambda_%7B1%7D+%5Clambda_%7B2%7D)

    a window with a score R greater than a certain value is considered a "corner"
