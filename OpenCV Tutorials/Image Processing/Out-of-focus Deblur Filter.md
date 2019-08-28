目的

在这篇文章中，你将会学到：

* 什么叫退化图像模型
* 什么是失去焦点图片的PSF（point spread function，扩散函数）
* 如何将一张模糊的图片恢复清晰
* 介绍维纳滤镜

Theory

> Note
> 
> The explanation is based on the books [79] and [256]. Also, you can refer to Matlab's tutorial Image Deblurring in Matlab and the article SmartDeblur.
>
> The out-of-focus image on this page is a real world image. The out-of-focus was achieved manually by camera optics.

What is a degradation image model?

Here is a mathematical model of the image degradation in frequency domain representation:

S=H⋅U+N

where S is a spectrum of blurred (degraded) image, U is a spectrum of original true (undegraded) image, H is a frequency response of point spread function (PSF), N is a spectrum of additive noise.

The circular PSF is a good approximation of out-of-focus distortion. Such a PSF is specified by only one parameter - radius R. Circular PSF is used in this work.

psf.png

Circular point spread function

How to restore a blurred image?

The objective of restoration (deblurring) is to obtain an estimate of the original image. The restoration formula in frequency domain is:

U′=Hw⋅S

where U′ is the spectrum of estimation of original image U, and Hw is the restoration filter, for example, the Wiener filter.

What is the Wiener filter?

The Wiener filter is a way to restore a blurred image. Let's suppose that the PSF is a real and symmetric signal, a power spectrum of the original true image and noise are not known, then a simplified Wiener formula is:

Hw=H|H|2+1SNR

where SNR is signal-to-noise ratio.

So, in order to recover an out-of-focus image by Wiener filter, it needs to know the SNR and R of the circular PSF.