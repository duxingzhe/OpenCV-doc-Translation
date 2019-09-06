目的

在本教程中，你将会学到：

* 径向结构张量
* 如何使用径向结构张量来预测各向异向图像的方向和相关性
* 通过径向结构张量使用单一本地方向来将各向异向图区分开来

理论

> 注意
> 
> 本文理论知识引用自参考书目[100]、[18]和[22s0]。对径向结构张量分析得比较好的书是参考书目[241]。同时，你可以参考结构张量的维基百科页面。
> 本教程中的各向异向图片取自真实图片。

什么是径向结构张量？

在数学上，径向结构张量（同样被称为矩矩阵、二阶矩张量、惯性张量等）是一个函数的径向展开矩阵。它代表了一个点特定临界的主要径向，各个角度之间的关联度。，径向结构张量被广泛应用在二维和三维的图像处理和机器视觉中的图像分离、运动检测、适应性过滤和本地图片特征检测等当中。

各向异向图片的重要特征包括了本身异向的方向和相关度。在这篇教程中，我们将介绍如何预测方向和相关度、如何通过径向结构张量使用单一本地方向来将各向异向图片区分开来。I

图像的径向结构张量是一个2x2对称矩阵。径向结构张量的特征值表明了图像本身的方向，特征向量则提供了相关型（对各向异向特征的衡量有一定帮助）。

对于一张图片Z的径向结构张量可以用如下式子表示：

![](http://latex.codecogs.com/gif.latex?J%20%3D%20%5Cbegin%7Bbmatrix%7D%20J_%7B11%7D%20%26%20J_%7B12%7D%20%5C%5C%20J_%7B12%7D%20%26%20J_%7B22%7D%20%5Cend%7Bbmatrix%7D)

其中![](http://latex.codecogs.com/gif.latex?J_%7B11%7D%20%3D%20M%5BZ_%7Bx%7D%5E%7B2%7D%5D) , ![](http://latex.codecogs.com/gif.latex?J_%7B22%7D%20%3D%20M%5BZ_%7By%7D%5E%7B2%7D%5D) , ![](http://latex.codecogs.com/gif.latex?J_%7B12%7D%20%3D%20M%5BZ_%7Bx%7DZ_%7By%7D%5D)张量的组成部分，M[]则是数学期望的符号（我们将认为这个符号能够在某一个窗口上取一个平均值），![](http://latex.codecogs.com/gif.latex?Z_x)和![](http://latex.codecogs.com/gif.latex?Z_y)是Z在x和y方向的展开式。

通过以下公式，我们能够计算出张量的特征值：

![](http://latex.codecogs.com/gif.latex?%5Clambda_%7B1%2C2%7D%20%3D%20J_%7B11%7D%20+%20J_%7B22%7D%20%5Cpm%20%5Csqrt%7B%28J_%7B11%7D%20-%20J_%7B22%7D%29%5E%7B2%7D%20+%204J_%7B12%7D%5E%7B2%7D%7D)

其中![](http://latex.codecogs.com/gif.latex?\lambda_1)是最大的特征值，![](http://latex.codecogs.com/gif.latex?\lambda_2)是最小的特征值。

如何用径向结构张量分析一张各向异向图的方向和相关度？

各向异向图的方向：

![](http://latex.codecogs.com/gif.latex?%5Calpha%20%3D%200.5arctg%5Cfrac%7B2J_%7B12%7D%7D%7BJ_%7B22%7D%20-%20J_%7B11%7D%7D)

相关性

![](http://latex.codecogs.com/gif.latex?C%20%3D%20%5Cfrac%7B%5Clambda_1%20-%20%5Clambda_2%7D%7B%5Clambda_1%20+%20%5Clambda_2%7D)

相关性的范围是0到1。对于自身方向（![](http://latex.codecogs.com/gif.latex?\lambda_2) = 0, ![](http://latex.codecogs.com/gif.latex?\lambda_1) > 0）是一，对于同向灰阶结构而言，（ ![](http://latex.codecogs.com/gif.latex?\lambda_1) = ![](http://latex.codecogs.com/gif.latex?\lambda_2) > 0）它是零。  it is zero.