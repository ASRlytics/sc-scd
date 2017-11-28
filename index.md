---
layout: page
title: {{ site.name }}
page_math: true
---
<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>

# Overview
This is my (Nikolaos - *Nikos* - Flemotomos a.k.a. "team" DeepSpeech) final project report for the class [Deep Learning and its Applications][class], as offered during the Fall semester of 2017-2018 at the [University of Southern California][usc]. 

# What is SCD?
Speaker Change Detection (SCD) can be defined as dividing an audio signal into multiple audio chunks such that each of them denotes a speaker homogeneous region. Any time interval in the signal containing only one speaker can be thought of as a 'speaker homogeneous region'. SCD is a very important issue in Speech Processing with various applications, such as Speaker Tracking (SCD + Speaker Verification) or Speaker Diarization (SCD + Speaker Clustering). In this work, time intervals of silence between speakers or overlapping speech were not taken into consideration. A hypothetical output of an SCD system is depicted in the following figure:

<center><img src="content/scd_eg.pdf"></center>
<i><font size="3">The mel-spectrogram of a signal where the timestamps of the true speaker turns (solid green lines) and the speaker turns predicted by a hypothetical Speaker Change Detector (dashed white lines) are denoted. The correct detections are denoted assuming that a reasonable tolerance margin is applied.</font></i>

# What are Siamese Neural Networks?

Siamese networks, as introduced in [Chopra et al. 2005][Chopra2005] and [Hadsell et al. 2006][Hadsell2006], aim at learning a (dis)similarity metric between two inputs, by comparing their outputs, as produced by two identical subnetworks (with the same architecture and weights). Originally, it was suggested that this "comparison" is done based on the euclidean distance between the subnetworks' outputs, but other approaches can be also taken, as discussed in the [Implementation and experiments](experiments.html) section.
  
# Main Results
Assuming that the desired behavior is a balance between missed detections and false alarms (errors of Type I and II), the best system that was trained yielded an $MDR=29.22\%$, $FAR=27.08\%$ and $F_1 = 68.08$

Here is a demo of the result for a small segment of the test set:

For extensive experimental results, as well as for details about the evaluation metrics used and their definitions, please refer to the [Implementation and experiments](experiments.html) section.

# Let's Have a *Deep*er Understanding...
In the following pages you can find details about
* [Related work](related_work.html)
* [Datasets, data preparation, and input pipeline](data.html)
* [Implementation and experiments](experiments.html)

[class]: https://csci599-dl.github.io
[usc]: https://www.usc.edu
[Chopra2005]: http://ieeexplore.ieee.org/abstract/document/1467314/
[Hadsell2006]: http://ieeexplore.ieee.org/document/1640964/
