目的

在本教程中，你将会学到：

* 什么是反向映射，他为什么那么重要
* 如何使用OpenCV的cv::calcBackProject函数来计算反向映射
* 通过OpenCV的cv::mixChannels函数来混合不同的色彩通道

理论

什么是反向映射？

* Back Projection is a way of recording how well the pixels of a given image fit the distribution of pixels in a histogram model.
* To make it simpler: For Back Projection, you calculate the histogram model of a feature and then use it to find this feature in an image.
* Application example: If you have a histogram of flesh color (say, a Hue-Saturation histogram ), then you can use it to find flesh color areas in an image:

How does it work?

* We explain this by using the skin example:
* Let's say you have gotten a skin histogram (Hue-Saturation) based on the image below. The histogram besides is going to be our model histogram (which we know represents a sample of skin tonality). You applied some mask to capture only the histogram of the skin area:

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