目的

在这篇文章中，你将会学到：

* 什么叫退化图像模型
* 什么是失去焦点图片的PSF（point spread function，点扩散函数）
* 如何将一张模糊的图片恢复清晰
* 介绍维纳滤镜

理论

> 注意
> 
> 本教程例子说明主要基于[79]和[256]两本书。同时你可以查阅Matlab的教程《图像去模糊》和文章《智能去模糊》。
>
> 本教程提及的失焦图片是一张现实世界图片。失焦图片是通过相机调节手动生成的。

什么是退化图像模型？

这是在图像频域表示的图像退化算法数学模型：

S=H⋅U+N

其中，S是模糊化（退化的）图像频谱，U是原来真实（未退化）的图片，H是点扩散函数（PSF）的频率反馈，N是额外噪声的频谱。

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