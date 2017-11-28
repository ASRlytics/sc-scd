---
layout: page
title:  "Datasets"
date:   2017-11-21
use_math: true
---
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# Datasets, Data Preparation, and Input Pipeline

### Training Phase

For the training of the system, utterances from [AMI Corpus][amicorpus], Fisher English ([Part 1][fisher1] and [Part 2][fisher2]), [1997 English Broadcast News HUB4][hub4], [ICSI Meeting Speech][icsi], [Librispeech][librispeech], [TED-LIUM][tedlium], and WSJ ([WSJ0][wsj0] and [WSJ1][wsj1]) were collected. After removing the silence, all the utterances were kept, together with the labels matching each utterance with a specific speaker. All the utterances were normalized to $16$kHz sampling frequency and $16$-bit precision. Out of those utterances, those smaller than $5$sec long were discarded, because very small utterances had probably great overlapping with neighboring utterances in case of a dialogue. All the speakers with less than $30$ utterances were also discarded, in order to ensure that there was a reasonable number of utterances from each speaker. Finally, only a snippet of $1.27$sec from the middle of each one of the remaining utterances was kept. That led to a total of ... segments $1.27$sec-long. For each one, the mel-spectrogram was extracted with a window shift of 10msec, where each window was center-aligned with the correspondisg frame, and with $128$ frequency bins, thus transforming each segment into a 2D representation (one-color image) of size $128\times128$. This set of spectrograms was split into a training and validation set according to a random $80-20$ speaker-based split, so that the same speakers did not appear in both the training and the validation sets, since the idea was to train a system with good generalization to non-seen speakers.

### Data Augmentation

In order to take into consideration multiple possible conditions, data augmentation took place as follows: For each utterance available, a new utterance was created by
* changing the speed of the audio by a factor randomly chosen in the range $[0.9, 1.1]$,
* adding reverberation by filtering the utterance with one of [96 simulated Room Impulse Responses](https://reverb2014.dereverberation.com/download.html), and
* adding white noise with Signal-to-Noise Ratio $(SNR)$ randomly chosen in the range $[10\text{dB}, 20\text{dB}]$

### Testing Phase
To test the performance of the trained system, an artificial dialogue was created using utterrances found in [TIMIT][timit] database. Specifically, utterances were randomly chosen without replacement from the TEST directory of the database. After removing the inter-speaker silence (so that a simple Voice Activity Detector could not yield any reliable results), the final testing recording consisted of $30$ minutes of speech with $656$ speaker turns.

The testing recording was split into consecutice overlapping segments of duration $1.27$sec and, after extracting the corresponding spectrograms, adjacent segments were compared throught the network to decide whether the segments belong to the same or different speakers. The exact approach is described in detail in the [Implementation and experiments](experiments.html) section.

### Minibatch Selection
For each minibatch $p$ pairs of segments for $N$ speakers were randomly chosen and those segments were additionally combined in a way that there was exactly one pair from each combination of 2 speakers. Thus, each minibatch contained $\frac{N(N-1)}{2}$ differnet-speaker pairs and $Np$ same-speaker pairs.

### Input Pipeline
Due to the huge number of possible combinations between different utterances spoken either by the same or by different speakers, the way the training data was fed in the network had to be chosen in a clever way, in order to ensure that lots of different combinations of speakers and segments were compared and that the approach taken was compatible with the resources available (GPU Nvidia 1080Ti, 32GB RAM). For creating a training batch, speakers were being randomly selected one at a time without replacement and all the available utterances spoken by the specific speaker were being loaded into memory while the total number of utterances was less than $U_{max}=50000$. Let's call this group of utterances $S$. Out of those segments, $M=800$ minibatches were being created, as previously described, with $N=9$ and $p=4$. The choice of those values for $N$ and $p$ led to minibatches of $72$ pairs with perfect balance between same-speaker and different-speaker pairs. This approach was repeated for $T=30$ times for each group $S_i$, while $G=10$ such groups were created ($i=1,2,\cdots,G$), leading to a total number of training pairs equal to $10\cdot30\cdot800\cdot72=17\,280\,000$. The pseudocode of the input pipeline algorithm is:

```
for i = 1,2,...,G:
  randomly load into memory a group S_i of all the utterances
   from as many speakers as possible such that |S_i| <= U_max 
  for t = 1,2,...,T:
    for m = 1,2,...,M:
      I]   randomly choose N speakers from S_i w/o replacement 
           and 2p utterances from each one of them
      II]  with those utterances create Np same-speaker 
           and N(N-1)/2 different-speaker pairs
      III] train the network for one epoch considering as 
           training set the just created set of M minibatches 
```
The GPU and the CPU could work in parallel so that while the network was trained on GPU, the next batch $S_i$ was being loaded.

The validation set - which was the same across the different groups $S_i$ and the $T$ iterations - was created the same way by choosing $U_{max}=10000$ and $M=100$, giving $100\cdot72=7200$ validation pairs. It is noted that by setting a fixed seed for the random generators, it was guaranteed that the same training and validation sets were used across experiments comparing different approaches.


[amicorpus]: http://groups.inf.ed.ac.uk/ami/corpus/
[fisher1]: https://catalog.ldc.upenn.edu/LDC2004S13
[fisher2]: https://catalog.ldc.upenn.edu/LDC2005S13
[hub4]: https://catalog.ldc.upenn.edu/ldc98s71
[icsi]: https://catalog.ldc.upenn.edu/LDC2004S02
[librispeech]: http://www.openslr.org/12
[tedlium]: http://www-lium.univ-lemans.fr/en/content/ted-lium-corpus
[wsj0]: https://catalog.ldc.upenn.edu/ldc93s6a
[wsj1]: https://catalog.ldc.upenn.edu/ldc94s13a
[timit]: https://catalog.ldc.upenn.edu/ldc93s1
