Goal

In this tutorial you will learn:

* what the PSF of a motion blur image is
* how to restore a motion blur image

Theory

For the degradation image model theory and the Wiener filter theory you can refer to the tutorial Out-of-focus Deblur Filter. On this page only a linear motion blur distortion is considered. The motion blur image on this page is a real world image. The blur was caused by a moving subject.

What is the PSF of a motion blur image?

The point spread function (PSF) of a linear motion blur distortion is a line segment. Such a PSF is specified by two parameters: LEN is the length of the blur and THETA is the angle of motion.

![](https://docs.opencv.org/4.1.0/motion_psf.png)

Point spread function of a linear motion blur distortion

How to restore a blurred image?

On this page the Wiener filter is used as the restoration filter, for details you can refer to the tutorial Out-of-focus Deblur Filter. In order to synthesize the Wiener filter for a motion blur case, it needs to specify the signal-to-noise ratio ( SNR), LEN and THETA of the PSF.