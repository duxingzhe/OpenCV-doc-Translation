目标

在这篇教程中，你将学到：

    * 使用cv::DescriptorExtractor接口来查找有关点的特征向量，特别是：
        * 使用cv:xfeatures2D::SURF和它的函数cv::xfeatures2d::SURF::compute来运行相关计算。
        * 使用cv::DescriptorMatcher来匹配特征向量
        * 使用cv:drawMatches来画出匹配值
        
    注意：
        你要使用OpenCV contrib模块来使用SURF特征（其他可选的有ORB, KAZE等特征）。