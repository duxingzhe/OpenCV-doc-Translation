目标

你将在本教程内学到：

* 使用OpenCV中的cv::morphologyEx函数来进行形态学的转换，例如：
    * 打孔
    * 闭合
    * 形态梯度
    * Top Hat（由于实在没有合适的中文翻译，故保留原文）
    * Black Hat

理论

> 注意
> 
> 以下内容来自Bradski and Kaehler的《OpenCV》入门一书。

在之前的教程中，我们介绍了两种基本的形态学操作：

* 侵蚀
* 扩张

基于这两项基本操作，我们能够对我们的图片进行更多的细致性操作。这里我们将用OpenCV来讨论5种操作：

Opening

* 这个操作是先扩张再侵蚀。

![](http://latex.codecogs.com/gif.latex?dst=open(src,element)=dilate(erode(src,element)))

* 可以用来去除一些小物体（假设小物体是处在黑暗区域的）
* 例如，观察一下下面的例子。左边的图形是原始图片，右边的图形是进行了Opening操作以后的图片。我们观察到小点已经消失了。

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_Opening.png)

Closing

* 这个操作是先侵蚀再扩张。

![](http://latex.codecogs.com/gif.latex?dst=close(src,element)=erode(dilate(src,element)))

* 可以用来去除一些小洞。

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_Closing.png)

Morphological Gradient

* 对于图片来说，他与扩张和侵蚀有些不同。

![](http://latex.codecogs.com/gif.latex?dst=morph_{grad}(src,element)=dilate(src,element)-erode(src,element))

* 可以用来临摹图像物体的轮廓，如下图所示：

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_Gradient.png)

Top Hat

* 可以对比一下输入图形和Opening操作后的图形。

![](http://latex.codecogs.com/gif.latex?dst=tophat(src,element)=src-open(src,element))

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_TopHat.png)

Black Hat

* 可以对比一下原始图形和经过Closing操作后的图形。

![](http://latex.codecogs.com/gif.latex?dst=blackhat(src,element)=close(src,element)-src)

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_BlackHat.png)
