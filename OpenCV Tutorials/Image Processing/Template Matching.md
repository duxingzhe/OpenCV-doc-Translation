目的

在这篇教程中，你将会学到：

* 使用OpenCV的matchTemplate()函数来搜索输入图像和一小块图片的联系。
* 使用OpenCV的minMaxLoc()函数在找到给定数组的最大值和最小值（以及他们的索引值）。

理论

什么是模板匹配？

模板匹配是寻找输入图片的一部分，用以确定是否与模板图片（一个小块）相匹配的技术。

当图片小块必须是一个矩形的时候，可能并不是所有的矩形都是合适的。在这个情况下，为了能够找到相匹配的模板，我们可以使用蒙版将小块的一部分隔离开来。

工作原理

* 我们需要两大部分：

    1.原图片(I)：这是为了和模板图片进行对比，以找到匹配的地方
    2.模板图片(T)：图片小块会和模板图片进行对比

我们的目的是为了能够检测到高度匹配的区域：

![](https://docs.opencv.org/Template_Matching_Template_Theory_Summary.jpg)

* 为了识别出匹配区域，我们通过滑动的方式比较模板图片和原图片：

![](https://docs.opencv.org/Template_Matching_Template_Theory_Sliding.jpg)

* 不停的滑动，我们将小块一次移动一个像素（从左往右，从上往下）。在每一个区域，都会将矩阵代入公式中进行计算用来确定和相关区域匹配度的“好坏”（与原图片相比较，小块图片对于这个区域的匹配度如何）。
* 对于I的区域和T之间的比较，你会将矩阵存储到矩阵R中。R矩阵中的每一个区域(x,y)包含了匹配矩阵：

![](https://docs.opencv.org/Template_Matching_Template_Theory_Result.jpg)

上面的图片是通过一个叫做TM_CCORR_NORMED的矩阵进行小块平移得到的结果R。最亮的部分表示是最匹配的部分。正如你看到的那样，用红色圆圈标出的地方是值最高的地方，所以在这一区域（矩形是以点为连接、长宽与图片小块一致生成的）被认为是最匹配的地方。

* 在实际应用中，我们使用minMaxLoc()函数来确定矩阵R中的最大值（或者最小值，这取决于我们使用匹配方法）。

蒙版的应用

* 如果匹配的时候我们需要蒙版的帮助，那么我们就需要三个部分：

    1.原图片(I)：这是为了和模板图片进行对比，以找到匹配的地方
    2.模板图片(T)：图片小块会和模板图片进行对比
    3.蒙版图片(M)：蒙版，灰阶图片，用蒙住模板图片

* 目前只有两种方法被认为是蒙版：TM_SQDIFF和TM_CCORR_NORMEDO（下面是所有OpenCV提供的匹配方法，会对这个知识进行详细解释）。
* 蒙板必须和模板有相同的维度
* 蒙版必须有CV_8U或者CV_32F的深度，和模板图片有同样的通道。在CV_8U情况下，蒙版的值会进行二值化处理，例如零和非零。在CV_32F情况下，值必须在[0..1]之间，模板像素必须与对应的蒙版像素值进行相乘。由于输入图片同样是CV_8UC3类型，蒙版同样会被认为是彩色图片。

![](https://docs.opencv.org/Template_Matching_Mask_Example.jpg)

OpenCV支持什么模型匹配方法？

这个问题提得非常好。OpenCV实现的方法都在matchTemplate()函数中。一共六种方法：

    1.method=TM_SQDIFF

    ![](http://latex.codecogs.com/gif.latex?R%28x%2Cy%29%3D%20%5Csum%20_%7Bx%27%2Cy%27%7D%20%28T%28x%27%2Cy%27%29-I%28x+x%27%2Cy+y%27%29%29%5E2)
    
    2. method=TM_SQDIFF_NORMED

    ![](http://latex.codecogs.com/gif.latex?R%28x%2Cy%29%3D%20%5Cfrac%7B%5Csum_%7Bx%27%2Cy%27%7D%20%28T%28x%27%2Cy%27%29-I%28x+x%27%2Cy+y%27%29%29%5E2%7D%7B%5Csqrt%7B%5Csum_%7Bx%27%2Cy%27%7DT%28x%27%2Cy%27%29%5E2%20%5Ccdot%20%5Csum_%7Bx%27%2Cy%27%7D%20I%28x+x%27%2Cy+y%27%29%5E2%7D%7D)

    3.method=TM_CCORR

    ![](http://latex.codecogs.com/gif.latex?R%28x%2Cy%29%3D%20%5Csum%20_%7Bx%27%2Cy%27%7D%20%28T%28x%27%2Cy%27%29%20%5Ccdot%20I%28x+x%27%2Cy+y%27%29%29)

    4.method=TM_CCORR_NORMED

    ![](http://latex.codecogs.com/gif.latex?R%28x%2Cy%29%3D%20%5Cfrac%7B%5Csum_%7Bx%27%2Cy%27%7D%20%28T%28x%27%2Cy%27%29%20%5Ccdot%20I%28x+x%27%2Cy+y%27%29%29%7D%7B%5Csqrt%7B%5Csum_%7Bx%27%2Cy%27%7DT%28x%27%2Cy%27%29%5E2%20%5Ccdot%20%5Csum_%7Bx%27%2Cy%27%7D%20I%28x+x%27%2Cy+y%27%29%5E2%7D%7D)

    5.method=TM_CCOEFF

    ![](http://latex.codecogs.com/gif.latex?R%28x%2Cy%29%3D%20%5Csum%20_%7Bx%27%2Cy%27%7D%20%28T%27%28x%27%2Cy%27%29%20%5Ccdot%20I%27%28x+x%27%2Cy+y%27%29%29)

    其中

    ![](http://latex.codecogs.com/gif.latex?%5Cbegin%7Barray%7D%7Bl%7D%20T%27%28x%27%2Cy%27%29%3DT%28x%27%2Cy%27%29%20-%201/%28w%20%5Ccdot%20h%29%20%5Ccdot%20%5Csum%20_%7Bx%27%27%2Cy%27%27%7D%20T%28x%27%27%2Cy%27%27%29%20%5C%5C%20I%27%28x+x%27%2Cy+y%27%29%3DI%28x+x%27%2Cy+y%27%29%20-%201/%28w%20%5Ccdot%20h%29%20%5Ccdot%20%5Csum%20_%7Bx%27%27%2Cy%27%27%7D%20I%28x+x%27%27%2Cy+y%27%27%29%20%5Cend%7Barray%7D)

    6.method=TM_CCOEFF_NORMED

    ![](http://latex.codecogs.com/gif.latex?R%28x%2Cy%29%3D%20%5Cfrac%7B%20%5Csum_%7Bx%27%2Cy%27%7D%20%28T%27%28x%27%2Cy%27%29%20%5Ccdot%20I%27%28x+x%27%2Cy+y%27%29%29%20%7D%7B%20%5Csqrt%7B%5Csum_%7Bx%27%2Cy%27%7DT%27%28x%27%2Cy%27%29%5E2%20%5Ccdot%20%5Csum_%7Bx%27%2Cy%27%7D%20I%27%28x+x%27%2Cy+y%27%29%5E2%7D%20%7D)