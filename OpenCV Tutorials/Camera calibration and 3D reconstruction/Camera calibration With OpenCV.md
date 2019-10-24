Cameras have been around for a long-long time. However, with the introduction of the cheap pinhole cameras in the late 20th century, they became a common occurrence in our everyday life. Unfortunately, this cheapness comes with its price: significant distortion. Luckily, these are constants and with a calibration and some remapping we can correct this. Furthermore, with calibration you may also determine the relation between the camera's natural units (pixels) and the real world units (for example millimeters).

Theory

For the distortion OpenCV takes into account the radial and tangential factors. For the radial factor one uses the following formula:

![](http://latex.codecogs.com/gif.latex?x_%7Bdistorted%7D%20%3D%20x%28%201%20+%20k_1%20r%5E2%20+%20k_2%20r%5E4%20+%20k_3%20r%5E6%29%20%5C%5C%20y_%7Bdistorted%7D%20%3D%20y%28%201%20+%20k_1%20r%5E2%20+%20k_2%20r%5E4%20+%20k_3%20r%5E6%29)

So for an undistorted pixel point at (x,y) coordinates, its position on the distorted image will be (![](http://latex.codecogs.com/gif.latex?x_%7Bdistorted%7D%2C%20y_%7Bdistorted%7D)). The presence of the radial distortion manifests in form of the "barrel" or "fish-eye" effect.

Tangential distortion occurs because the image taking lenses are not perfectly parallel to the imaging plane. It can be represented via the formulas:

![](http://latex.codecogs.com/gif.latex?x_%7Bdistorted%7D%20%3D%20x%20+%20%5B%202p_1xy%20+%20p_2%28r%5E2+2x%5E2%29%5D%20%5C%5C%20y_%7Bdistorted%7D%20%3D%20y%20+%20%5B%20p_1%28r%5E2+%202y%5E2%29+%202p_2xy%5D)

So we have five distortion parameters which in OpenCV are presented as one row matrix with 5 columns:

![](http://latex.codecogs.com/gif.latex?distortion%5C_coefficients%3D%28k_1%20%5Chspace%7B10pt%7D%20k_2%20%5Chspace%7B10pt%7D%20p_1%20%5Chspace%7B10pt%7D%20p_2%20%5Chspace%7B10pt%7D%20k_3%29)

Now for the unit conversion we use the following formula:

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