目的

在本教程中，你将会学到：

* 什么是反向映射，他为什么那么重要
* 如何使用OpenCV的cv::calcBackProject函数来计算反向映射
* 通过OpenCV的cv::mixChannels函数来混合不同的色彩通道

理论

什么是反向映射？

* 反向映射是一种能够在直方图模式下显示给定图片的像素与像素分布之间的相似程度的算法。
* 简而言之：对于反向映射，你可以将一个图形特征进行直方图显示，并通过它在一个图像中找到某种特征。
* 应用层例子：如果你在肉色中显示直方图（或者说色调——饱和度直方图），你就可以通过反向映射的方式找到图像中的皮肤：

他是如何工作的？

* 我们通过检测皮肤的例子来说明情况：
* 假如，你获得了一张肉色（色调饱和度）图片，就像下图这样的。我们的标准直方图就是旁边所展示的那样（我们由此可以知道，这是皮肤调性图片样例）。你可以用一些蒙版来获得只属于皮肤区域的直方图：

![](https://docs.opencv.org/4.1.0/Back_Projection_Theory0.jpg)
T0

![](https://docs.opencv.org/4.1.0/Back_Projection_Theory1.jpg)
T1

* Now, let's imagine that you get another hand image (Test Image) like the one below: (with its respective histogram):

![](https://docs.opencv.org/4.1.0/Back_Projection_Theory2.jpg)
T2
![](https://docs.opencv.org/4.1.0/Back_Projection_Theory3.jpg)
T3

* What we want to do is to use our model histogram (that we know represents a skin tonality) to detect skin areas in our Test Image. Here are the steps
    
    1.In each pixel of our Test Image (i.e. p(i,j) ), collect the data and find the correspondent bin location for that pixel (i.e. (hi,j,si,j) ).
    2.Lookup the model histogram in the correspondent bin - (hi,j,si,j) - and read the bin value.
    3.Store this bin value in a new image (BackProjection). Also, you may consider to normalize the model histogram first, so the output for the Test Image can be visible for you.
    4.Applying the steps above, we get the following BackProjection image for our Test Image:

    ![](https://docs.opencv.org/4.1.0/Back_Projection_Theory4.jpg)

    5.In terms of statistics, the values stored in BackProjection represent the probability that a pixel in Test Image belongs to a skin area, based on the model histogram that we use. For instance in our Test image, the brighter areas are more probable to be skin area (as they actually are), whereas the darker areas have less probability (notice that these "dark" areas belong to surfaces that have some shadow on it, which in turns affects the detection).