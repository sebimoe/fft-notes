
# fft-notes

Work in progress, below is what the notes may contain in the future.

  - applying fft to audio samples
  - generating spectrograms from complex fft result
  - converting complex fft result back to audio
  - generating audio from spectrograms
  - (avoiding) problems relating to phase-shift and bin alignment
  - notes on re-creating human speech
  - possibly notes on error functions for training audio neural nets


## Scope

This is only a brief set of notes expanded as I find some information and find it useful.

Processing audio using FFT (and related functions) finds many applications, these notes focus on two stated below.


#### Generating visual spectrograms

Generating visually pleasing spectrograms, and data-dense spectrograms.


#### Reversible transformation for neural networks

Transforming continouous audio sample stream into simple (as small as possoble and least frequent possible) frequency-domain data frames continouously, in hope to recreate similarly sounding audio from the stream of frequency-domain frames.


## Reference


### https://en.wikipedia.org/wiki/Short-time_Fourier_transform

> The short-time Fourier transform (STFT), is a -related transform used to determine the sinusoidal frequency and phase content of local sections of a signal as it changes over time.[1] In practice, the procedure for computing STFTs is to divide a longer time signal into shorter segments of equal length and then compute the Fourier transform separately on each shorter segment. This reveals the Fourier spectrum on each shorter segment. One then usually plots the changing spectra as a function of time.

> In the discrete time case, the data to be transformed could be broken up into chunks or frames (which usually overlap each other, to reduce artifacts at the boundary). Each chunk is Fourier transformed, and the complex result is added to a matrix, which records magnitude and phase for each point in time and frequency.

> The magnitude squared of the STFT yields the spectrogram representation of the Power Spectral Density.

Magnitude squared = real^2 + im^2 _= (sqrt(real^2 + im^2))^2_


### https://en.wikipedia.org/wiki/Lapped_transform

Related to "which usually overlap each other, to reduce artifacts at the boundary" above.

> In signal processing, a lapped transform is a type of linear discrete block transformation where the basis functions of the transformation overlap the block boundaries, yet the number of coefficients overall resulting from a series of overlapping block transforms remains the same as if a non-overlapping block transform had been used.

> Lapped transforms substantially reduce the blocking artifacts that otherwise occur with block transform coding techniques, in particular those using the discrete cosine transform. The best known example is the modified discrete cosine transform used in the MP3, Vorbis, AAC, and Opus audio codecs.


### https://en.wikipedia.org/wiki/Modified_discrete_cosine_transform


> The modified discrete cosine transform (MDCT) is a lapped transform based on the type-IV discrete cosine transform (DCT-IV), with the additional property of being lapped: it is designed to be performed on consecutive blocks of a larger dataset, where subsequent blocks are overlapped so that the last half of one block coincides with the first half of the next block.

> This overlapping, in addition to the energy-compaction qualities of the DCT, makes the MDCT especially attractive for signal compression applications, since it helps to avoid artifacts stemming from the block boundaries.

> As a result of these advantages, the MDCT is employed in most modern lossy audio formats, including MP3, AC-3, Vorbis, Windows Media Audio, ATRAC, Cook, AAC, Opus, and LDAC.

> As a lapped transform, the MDCT is a bit unusual compared to other Fourier-related transforms in that it has half as many outputs as inputs (instead of the same number).

> The inverse MDCT is known as the IMDCT. Because there are different numbers of inputs and outputs, at first glance it might seem that the MDCT should not be invertible. However, perfect invertibility is achieved by adding the overlapped IMDCTs of subsequent overlapping blocks, causing the errors to cancel and the original data to be retrieved; this technique is known as time-domain aliasing cancellation (TDAC).

#### Window functions

> In typical signal-compression applications, the transform properties are further improved by using a window function wn (n = 0, ..., 2N−1) that is multiplied with xn and yn in the MDCT and IMDCT formulas, above, in order to avoid discontinuities at the n = 0 and 2N boundaries by making the function go smoothly to zero at those points. (That is, we window the data before the MDCT and after the IMDCT.)

> The transform remains invertible (that is, TDAC works), for a symmetric window w{n} = w{2N−1−n}, as long as w satisfies the Princen-Bradley condition: w_{n}^2+w_{n+N}^2 =1

where N is the size of output (half of input size).

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/MDCT_WF.png/551px-MDCT_WF.png" height="320"/> https://commons.wikimedia.org/wiki/File:MDCT_WF.png

 - blue (Cosine, used in MP3, AC3, AAC)
 - green (Kaiser-Bessel-Derived, optional used in AAC, dark greenː α = 4, light greenː α = 6)
 - red (Sine Cosine, used in Vorbis)
