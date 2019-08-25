Goal

In this tutorial you will learn how to:

* Use the OpenCV function matchTemplate() to search for matches between an image patch and an input image
* Use the OpenCV function minMaxLoc() to find the maximum and minimum values (as well as their positions) in a given array.

Theory

What is template matching?

Template matching is a technique for finding areas of an image that match (are similar) to a template image (patch).

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

R(x,y)=∑x′,y′(T(x′,y′)−I(x+x′,y+y′))2
    
    2. method=TM_SQDIFF_NORMED

R(x,y)=∑x′,y′(T(x′,y′)−I(x+x′,y+y′))2∑x′,y′T(x′,y′)2⋅∑x′,y′I(x+x′,y+y′)2−−−−−−−−−−−−−−−−−−−−−−−−−−−−−−−−√

    3.method=TM_CCORR

R(x,y)=∑x′,y′(T(x′,y′)⋅I(x+x′,y+y′))

    4.method=TM_CCORR_NORMED

R(x,y)=∑x′,y′(T(x′,y′)⋅I(x+x′,y+y′))∑x′,y′T(x′,y′)2⋅∑x′,y′I(x+x′,y+y′)2−−−−−−−−−−−−−−−−−−−−−−−−−−−−−−−−√

    5.method=TM_CCOEFF

R(x,y)=∑x′,y′(T′(x′,y′)⋅I′(x+x′,y+y′))

    where

T′(x′,y′)=T(x′,y′)−1/(w⋅h)⋅∑x′′,y′′T(x′′,y′′)I′(x+x′,y+y′)=I(x+x′,y+y′)−1/(w⋅h)⋅∑x′′,y′′I(x+x′′,y+y′′)

    6.method=TM_CCOEFF_NORMED

R(x,y)=∑x′,y′(T′(x′,y′)⋅I′(x+x′,y+y′))∑x′,y′T′(x′,y′)2⋅∑x′,y′I′(x+x′,y+y′)2−−−−−−−−−−−−−−−−−−−−−−−−−−−−−−−−−√