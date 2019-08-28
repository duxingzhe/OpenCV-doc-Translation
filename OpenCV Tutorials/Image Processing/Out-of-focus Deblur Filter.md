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

环形点扩散函数是对失焦扭曲计算的一种比较好的估算方法。这个估算方法只有一个变量——半径，R。环形点扩散函数是本教程中用到的例子。

![](https://docs.opencv.org/4.1.0/psf.png)

环形点扩散函数

如何恢复一张被模糊滤镜处理过的函数？

恢复物体（去模糊）的方法是去估计原来图片的像素值。在图像的频域，恢复函数为：

![](http://latex.codecogs.com/gif.latex?U%27%20%3D%20H_w%5Ccdot%20S)

其中U′是原来的图片U的预计频谱，而![](http://latex.codecogs.com/gif.latex?H_w)恢复滤镜，如维纳滤镜。

What is the Wiener filter?

The Wiener filter is a way to restore a blurred image. Let's suppose that the PSF is a real and symmetric signal, a power spectrum of the original true image and noise are not known, then a simplified Wiener formula is:

Hw=H|H|2+1SNR

where SNR is signal-to-noise ratio.

So, in order to recover an out-of-focus image by Wiener filter, it needs to know the SNR and R of the circular PSF.