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

Based on these two we can effectuate more sophisticated transformations to our images. Here we discuss briefly 5 operations offered by OpenCV:

Opening

* It is obtained by the erosion of an image followed by a dilation.

![](http://latex.codecogs.com/gif.latex?dst=open(src,element)=dilate(erode(src,element)))

* Useful for removing small objects (it is assumed that the objects are bright on a dark foreground)
* For instance, check out the example below. The image at the left is the original and the image at the right is the result after applying the opening transformation. We can observe that the small dots have disappeared.

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_Opening.png)

Closing

* It is obtained by the dilation of an image followed by an erosion.

![](http://latex.codecogs.com/gif.latex?dst=close(src,element)=erode(dilate(src,element)))

* Useful to remove small holes (dark regions).

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_Closing.png)

Morphological Gradient

* It is the difference between the dilation and the erosion of an image.

![](http://latex.codecogs.com/gif.latex?dst=morph_{grad}(src,element)=dilate(src,element)-erode(src,element))

* It is useful for finding the outline of an object as can be seen below:

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_Gradient.png)

Top Hat

* It is the difference between an input image and its opening.

![](http://latex.codecogs.com/gif.latex?dst=tophat(src,element)=src-open(src,element))

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_TopHat.png)

Black Hat

* It is the difference between the closing and its input image

![](http://latex.codecogs.com/gif.latex?dst=blackhat(src,element)=close(src,element)-src)

![](https://docs.opencv.org/4.1.0/Morphology_2_Tutorial_Theory_BlackHat.png)
