目标

这篇教程中，你将会学到：

* 什么是运动模糊图中的点扩散函数
* 如何让运动模糊图变得清晰

理论

对于退化图形模型理论和维纳滤镜理论，你可以查看失焦去模糊滤镜。在这篇教程中，我们只考虑线性运动模糊恢复算法。本文中的线性模糊图像是我们亲自拍照获得的。模糊的原因是因为物体的运动。

What is the PSF of a motion blur image?

The point spread function (PSF) of a linear motion blur distortion is a line segment. Such a PSF is specified by two parameters: LEN is the length of the blur and THETA is the angle of motion.

![](https://docs.opencv.org/4.1.0/motion_psf.png)

Point spread function of a linear motion blur distortion

How to restore a blurred image?

On this page the Wiener filter is used as the restoration filter, for details you can refer to the tutorial Out-of-focus Deblur Filter. In order to synthesize the Wiener filter for a motion blur case, it needs to specify the signal-to-noise ratio ( SNR), LEN and THETA of the PSF.