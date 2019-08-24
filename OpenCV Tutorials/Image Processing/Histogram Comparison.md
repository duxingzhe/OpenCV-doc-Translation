目标

在本教程，你将会学到：

* 使用cv::compareHist给出一个数字变量，用来表达两个直方图之间的相似度。
* 使用不同的矩阵来比较直方图。

理论

* 为了比较两个直方图（H1和H2）首先我们要一个矩阵（d(H1,H2)）来表示两者之间的联系。
* OpenCV通过cv::compareHist函数来实现直方图之间的比较。他有四个不同的矩阵类型来计算相似度：

    1.系数( CV_COMP_CORREL )

    ![](http://latex.codecogs.com/gif.latex?d%28H_1%2CH_2%29%20%3D%20%5Cfrac%7B%5Csum_I%20%28H_1%28I%29%20-%20%5Cbar%7BH_1%7D%29%20%28H_2%28I%29%20-%20%5Cbar%7BH_2%7D%29%7D%7B%5Csqrt%7B%5Csum_I%28H_1%28I%29%20-%20%5Cbar%7BH_1%7D%29%5E2%20%5Csum_I%28H_2%28I%29%20-%20%5Cbar%7BH_2%7D%29%5E2%7D%7D)

    where

    ![](http://latex.codecogs.com/gif.latex?%5Cbar%7BH_k%7D%20%3D%20%5Cfrac%7B1%7D%7BN%7D%20%5Csum%20_J%20H_k%28J%29)

    and N is the total number of histogram bins.
    2.Chi-Square ( CV_COMP_CHISQR )

    ![](http://latex.codecogs.com/gif.latex?d%28H_1%2CH_2%29%20%3D%20%5Csum%20_I%20%5Cfrac%7B%5Cleft%28H_1%28I%29-H_2%28I%29%5Cright%29%5E2%7D%7BH_1%28I%29%7D)

    3.Intersection ( method=CV_COMP_INTERSECT )

    ![](http://latex.codecogs.com/gif.latex?d%28H_1%2CH_2%29%20%3D%20%5Csum%20_I%20%5Cmin%20%28H_1%28I%29%2C%20H_2%28I%29%29)

    4.Bhattacharyya distance ( CV_COMP_BHATTACHARYYA )

    ![](http://latex.codecogs.com/gif.latex?d%28H_1%2CH_2%29%20%3D%20%5Csqrt%7B1%20-%20%5Cfrac%7B1%7D%7B%5Csqrt%7B%5Cbar%7BH_1%7D%20%5Cbar%7BH_2%7D%20N%5E2%7D%7D%20%5Csum_I%20%5Csqrt%7BH_1%28I%29%20%5Ccdot%20H_2%28I%29%7D%7D)