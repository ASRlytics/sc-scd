---
layout: page
title:  "Related Work"
date:   2017-11-21
use_math: true
---
<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>

# Related Work
The traditional approach for Speaker Change Detection has been to extract the Mel-Frequency Cepstrum Coefficients (MFCCs are usually $13$-dimensional feature vectors representing a $20-30$msec window) of the signal and measure the distance between two consecutive windows using some metric such as the metric based on Bayesian Information Criterion (ΔBIC), the Generalized Likelihood Ratio (GLR), or the Kullback-Leibler divergence (KL or KL2). In such a way, a distance curve is obtained, the peaks of which - probably after some kind of smoothing - denote the moments where a speaker turn has been happened ([Kotti et al. 2008][Kotti2008], [Anguera et al. 2012][Anguera2012]). More recent approaches include the use of i-vectors and Probabilistic Linear Discriminant Analysis (PLDA) ([Sell and Garcia-Romero 2014][Sell2014]).

During the last few years, there is an interest to apply Deep Learning techniques in order to train a network to learn whether a segment contains a speaker turn ([Gupta 2015][Gupta2015], [Hruz and Zajic 2017][Hruz2017]) or whether two consecutive segments belong to the same speaker or not ([Chen and Salman 2011][Chen2011], [Yella and Stolcke 2015][Yella2015], [Jati and Georgiou 2017][Jati2017], [Garcia-Romero et al. 2017][Romero2017]), which is the approach followed in this work. For example, in [Chen and Salman 2011][Chen2011] a siamese network with autoencoders is trained based on a loss function which is the sum of the reconstruction loss of each segment plus the contrastive loss between them. The idea is to discard the linguistic information and only keep the speaker-specific information. In [Yella and Stolcke 2015][Yella2015] different neural network architectures, among which feed-forward siamese networks, for Speaker Diarization are compared. In [Garcia-Romero et al. 2017][Romero2017], another siamese architecture is suggested with time-delay and temporal pooling, where the netowrk also learns the parameters of the loss function. In [Jati and Georgiou 2017][Jati2017], the authors propose an unsupervised technique where an autoencoder is trained based on the reconstruction loss between consecutive segments which are assumed to belong to the same speaker most of the time.

All the approaches presented above treat initially the speech signal as a concatenation of low-dimensional MFCCs. However, this preprocessing may lead to information loss as far as speaker characteristics are concerned. In order to have a 'raw' form of the signal, we can represent each segment by its spectrogram (2D representation similar to images) and use convolutional kernels to learn all the relevant features ([Lukic et al. 2016][Lukic2016], [Cyrta et al. 2017][Cyrta2017], [Hruz and Zajic 2017][Hruz2017]).

In this work, it is suggested that the strength of the Convolutional Networks is used to tackle the problem of Speaker Change Detection, by applying them in a siamese architecture framework.

[Kotti2008]: http://www.sciencedirect.com/science/article/pii/S016516840700391X
[Anguera2012]: http://ieeexplore.ieee.org/abstract/document/6135543/
[Sell2014]: http://ieeexplore.ieee.org/abstract/document/7078610/
[Gupta2015]: http://ieeexplore.ieee.org/abstract/document/7178806/
[Hruz2017]: http://ieeexplore.ieee.org/abstract/document/7953097/
[Chen2011]: http://ieeexplore.ieee.org/abstract/document/6026951/
[Yella2015]: http://www.isca-speech.org/archive/interspeech_2015/i15_3026.html
[Jati2017]: http://www.isca-speech.org/archive/Interspeech_2017/pdfs/1650.PDF
[Romero2017]: http://ieeexplore.ieee.org/document/7953094/
[Peddinti2015]: http://www.isca-speech.org/archive/interspeech_2015/i15_3214.html
[Lukic2016]: http://ieeexplore.ieee.org/document/7738816/
[Cyrta2017]: https://link.springer.com/chapter/10.1007/978-3-319-67220-5_10
