目标

在这篇教程中，你将学到：

    * 使用cv::FeatureDetector接口来找到兴趣点，特别的：
        * 使用cv::xfeatures2d::SURF和它的函数cv::xfeatures2d::SURF::detect来进行检测
        * 使用cv::drawKeypoints来画出发现的关键点。
        
    注意：
        你要使用OpenCV contrib模块来使用SURF特征（其他可选的有ORB, KAZE等特征）。