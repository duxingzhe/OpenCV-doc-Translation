目的

在这篇教程中，你将会学到：

* 使用OpenCV的matchTemplate()函数来搜索输入图像和一小块图片的联系。
* 使用OpenCV的minMaxLoc()函数在找到给定数组的最大值和最小值（以及他们的索引值）。

理论

什么是模板匹配？

模板匹配是寻找输入图片的一部分，用以确定是否与模板图片（一个小块）相匹配的技术。

While the patch must be a rectangle it may be that not all of the rectangle is relevant. In such a case, a mask can be used to isolate the portion of the patch that should be used to find the match.

How does it work?

* We need two primary components:

    1.Source image (I): The image in which we expect to find a match to the template image
    2.Template image (T): The patch image which will be compared to the template image

our goal is to detect the highest matching area:

Template_Matching_Template_Theory_Summary.jpg

* To identify the matching area, we have to compare the template image against the source image by sliding it:

Template_Matching_Template_Theory_Sliding.jpg

* By sliding, we mean moving the patch one pixel at a time (left to right, up to down). At each location, a metric is calculated so it represents how "good" or "bad" the match at that location is (or how similar the patch is to that particular area of the source image).
* For each location of T over I, you store the metric in the result matrix R. Each location (x,y) in R contains the match metric:

Template_Matching_Template_Theory_Result.jpg

the image above is the result R of sliding the patch with a metric TM_CCORR_NORMED. The brightest locations indicate the highest matches. As you can see, the location marked by the red circle is probably the one with the highest value, so that location (the rectangle formed by that point as a corner and width and height equal to the patch image) is considered the match.

* In practice, we locate the highest value (or lower, depending of the type of matching method) in the R matrix, using the function minMaxLoc()

How does the mask work?

* If masking is needed for the match, three components are required:

    1.Source image (I): The image in which we expect to find a match to the template image
    2.Template image (T): The patch image which will be compared to the template image
    3.Mask image (M): The mask, a grayscale image that masks the template

* Only two matching methods currently accept a mask: TM_SQDIFF and TM_CCORR_NORMED (see below for explanation of all the matching methods available in opencv).
* The mask must have the same dimensions as the template
* The mask should have a CV_8U or CV_32F depth and the same number of channels as the template image. In CV_8U case, the mask values are treated as binary, i.e. zero and non-zero. In CV_32F case, the values should fall into [0..1] range and the template pixels will be multiplied by the corresponding mask pixel values. Since the input images in the sample have the CV_8UC3 type, the mask is also read as color image.

Template_Matching_Mask_Example.jpg

Which are the matching methods available in OpenCV?

Good question. OpenCV implements Template matching in the function matchTemplate(). The available methods are 6:

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

    where

    ![](http://latex.codecogs.com/gif.latex?%5Cbegin%7Barray%7D%7Bl%7D%20T%27%28x%27%2Cy%27%29%3DT%28x%27%2Cy%27%29%20-%201/%28w%20%5Ccdot%20h%29%20%5Ccdot%20%5Csum%20_%7Bx%27%27%2Cy%27%27%7D%20T%28x%27%27%2Cy%27%27%29%20%5C%5C%20I%27%28x+x%27%2Cy+y%27%29%3DI%28x+x%27%2Cy+y%27%29%20-%201/%28w%20%5Ccdot%20h%29%20%5Ccdot%20%5Csum%20_%7Bx%27%27%2Cy%27%27%7D%20I%28x+x%27%27%2Cy+y%27%27%29%20%5Cend%7Barray%7D)

    6.method=TM_CCOEFF_NORMED

    ![](http://latex.codecogs.com/gif.latex?R%28x%2Cy%29%3D%20%5Cfrac%7B%20%5Csum_%7Bx%27%2Cy%27%7D%20%28T%27%28x%27%2Cy%27%29%20%5Ccdot%20I%27%28x+x%27%2Cy+y%27%29%29%20%7D%7B%20%5Csqrt%7B%5Csum_%7Bx%27%2Cy%27%7DT%27%28x%27%2Cy%27%29%5E2%20%5Ccdot%20%5Csum_%7Bx%27%2Cy%27%7D%20I%27%28x+x%27%2Cy+y%27%29%5E2%7D%20%7D)