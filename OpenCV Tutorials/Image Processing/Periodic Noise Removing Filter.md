Goal

In this tutorial you will learn:

* how to remove periodic noise in the Fourier domain

Theory

> Note
> 
> The explanation is based on the book [79]. The image on this page is a real world image.

Periodic noise produces spikes in the Fourier domain that can often be detected by visual analysis.

How to remove periodic noise in the Fourier domain?

Periodic noise can be reduced significantly via frequency domain filtering. On this page we use a notch reject filter with an appropriate radius to completely enclose the noise spikes in the Fourier domain. The notch filter rejects frequencies in predefined neighborhoods around a center frequency. The number of notch filters is arbitrary. The shape of the notch areas can also be arbitrary (e.g. rectangular or circular). On this page we use three circular shape notch reject filters. Power spectrum densify of an image is used for the noise spike’s visual detection.