摄像机已经发明面世有一段时间了。然而直到20世纪后期针孔摄像机发明之后，摄像机才进入我们的日常生活当中。不幸的是，便宜也意味着性能不会太好：有明显的扭曲。幸运的是，他们通常是一个定值，并且通过修正和简单的重映射，我们就能修复它。另外，通过修正，你可以确定相机自然单位（像素）和真实世界单位（比如，毫米）之间的换算关系。

理论

对于扭曲，OpenCV通过半径和正切值做为参数来进行修正。对于半径参数，可以用以下公式：

![](http://latex.codecogs.com/gif.latex?x_%7Bdistorted%7D%20%3D%20x%28%201%20+%20k_1%20r%5E2%20+%20k_2%20r%5E4%20+%20k_3%20r%5E6%29%20%5C%5C%20y_%7Bdistorted%7D%20%3D%20y%28%201%20+%20k_1%20r%5E2%20+%20k_2%20r%5E4%20+%20k_3%20r%5E6%29)

对于在未扭曲的图像中像素点的坐标值为（x,y）,其在已经扭曲的图像中对应的坐标将是 (![](http://latex.codecogs.com/gif.latex?x_%7Bdistorted%7D%2C%20y_%7Bdistorted%7D)). 辐射扭曲的效果就好像“圆柱”和“鱼眼”特效一样。

正切扭曲是在图像在晶状体面前的时候不再和成像面平行了。它的效果由下式表达：

![](http://latex.codecogs.com/gif.latex?x_%7Bdistorted%7D%20%3D%20x%20+%20%5B%202p_1xy%20+%20p_2%28r%5E2+2x%5E2%29%5D%20%5C%5C%20y_%7Bdistorted%7D%20%3D%20y%20+%20%5B%20p_1%28r%5E2+%202y%5E2%29+%202p_2xy%5D)

因此我们就有了五个扭曲变量，这五个变量在OpenCV中用一行五列的矩阵表示：

![](http://latex.codecogs.com/gif.latex?distortion%5C_coefficients%3D%28k_1%20%5Chspace%7B10pt%7D%20k_2%20%5Chspace%7B10pt%7D%20p_1%20%5Chspace%7B10pt%7D%20p_2%20%5Chspace%7B10pt%7D%20k_3%29)

为了有统一的变换，我们有以下公式：

![](http://latex.codecogs.com/gif.latex?%5Cleft%20%5B%20%5Cbegin%7Bmatrix%7D%20x%20%5C%5C%20y%20%5C%5C%20w%20%5Cend%7Bmatrix%7D%20%5Cright%20%5D%20%3D%20%5Cleft%20%5B%20%5Cbegin%7Bmatrix%7D%20f_x%20%26%200%20%26%20c_x%20%5C%5C%200%20%26%20f_y%20%26%20c_y%20%5C%5C%200%20%26%200%20%26%201%20%5Cend%7Bmatrix%7D%20%5Cright%20%5D%20%5Cleft%20%5B%20%5Cbegin%7Bmatrix%7D%20X%20%5C%5C%20Y%20%5C%5C%20Z%20%5Cend%7Bmatrix%7D%20%5Cright%20%5D)

Here the presence of w is explained by the use of homography coordinate system (and w=Z). The unknown parameters are ![](http://latex.codecogs.com/gif.latex?f_x) and ![](http://latex.codecogs.com/gif.latex?f_y) (camera focal lengths) and ![](http://latex.codecogs.com/gif.latex?(c_x,c_y)) which are the optical centers expressed in pixels coordinates. If for both axes a common focal length is used with a given a aspect ratio (usually 1), then ![](http://latex.codecogs.com/gif.latex?f_y=f_x*a) and in the upper formula we will have a single focal length f. The matrix containing these four parameters is referred to as the camera matrix. While the distortion coefficients are the same regardless of the camera resolutions used, these should be scaled along with the current resolution from the calibrated resolution.

The process of determining these two matrices is the calibration. Calculation of these parameters is done through basic geometrical equations. The equations used depend on the chosen calibrating objects. Currently OpenCV supports three types of objects for calibration:

* Classical black-white chessboard
* Symmetrical circle pattern
* Asymmetrical circle pattern

Basically, you need to take snapshots of these patterns with your camera and let OpenCV find them. Each found pattern results in a new equation. To solve the equation you need at least a predetermined number of pattern snapshots to form a well-posed equation system. This number is higher for the chessboard pattern and less for the circle ones. For example, in theory the chessboard pattern requires at least two snapshots. However, in practice we have a good amount of noise present in our input images, so for good results you will probably need at least 10 good snapshots of the input pattern in different positions.

Goal

The sample application will:

* Determine the distortion matrix
* Determine the camera matrix
* Take input from Camera, Video and Image file list
* Read configuration from XML/YAML file
* Save the results into XML/YAML file
* Calculate re-projection error