# Audio/Sound
1. Real-world signal ---> analog or continuous
2. Can be a single frequency (pure tone)
3. Sound is made by changes in air pressure at frequencies that are audible to humans.
4. The amplitude of a sound is the sound pressure level at any given instant and is measured in decibels (dB).
5. We perceive the amplitude as loudness.
- a normal speaking voice is under 60 dB,
- a rock concert can be at around 125 dB, pushing the limits of human hearing.
6. Audio can be stored in analog + digital format
7. For digital audio signals, 0 dB is the loudest possible amplitude, while all other amplitudes are negative.
8. Every -6 dB is a halving of the amplitude, and anything below -60 dB is generally inaudible unless we really crank up the volume.  
9. Analog Storage Medium: Vinyl records or cassette tapes
10. In most modern systems, audio is converted from analog ---> digital (ADC): for ease of storage, transmission, and manipulation (digital processing)

# Audio Channels

1. Channel:
2. Mono: has a single channel of audio --> apt for speech recognition or music genre classification
3. Stereo: contains two separate channels of audio, typically one for the left and one for the right ---> tasks that require an understanding of the spatial location of sound sources, like sound source localization or certain types of sound scene analysis
4. Multi-channel audio (5.1, 7.1, etc): 5.1 --> 5 speakers + 1 sub-woofer (120 Hz) 

# Binaural

Method of recording or reproducing sound that uses two microphones, arranged with the intent to create a 3-D stereo sound sensation for the listener.

# Analog, Discrete, and Digital Signals
Analog signal: Continuous, smooth, uninterrupted

Discrete signal: Analog signals sampled at specific intervals ( hence exist at distinct points in time), but the amplitude of the signal can still take any value.

Digital signal: Discrete signals whose amplitude can take on only a finite number of values (usually integers)

Note: The process of converting an audio signal from the analog domain to digital involves two main steps: Sampling and Quantization (pulse code modulation, etc)

# ADC

1. Successive Approximation Register (SAR) ADC:
2. Pulse Coded Modulation (PCM):
3. The "best" ADC depends on the specific requirements of the application i.e. required resolution, speed, power consumption, and cost.


# Sampling, Upsampling, Downsampling

1. Sampling: analog-to-digital conversion of audio signals. It involves measuring the amplitude of the analog waveform at regular time intervals and converting those measurements into digital data ---> output: digital discrete signal
![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/fc73bde3-7611-4594-96aa-e92bf5fa73ac)



3. Sampling Rate: number of samples of audio taken per second during the process of analog-to-digital conversion.
  - High-resolution Audio: 192 kHz
  - Common sampling rate used in training speech models is 16 kHz.
  - The audible frequencies in human speech are below 8 kHz, hence sampling speech at 16 kHz is sufficient. Using a higher sampling rate will not capture more information and merely leads to an increase in the computational cost of processing such files.
  - sampling audio at too low a sampling rate will result in information loss. Speech sampled at 8 kHz will sound muffled, as the higher frequencies cannot be captured at this rate.
  - custom audio data to fine-tune a pre-trained model, the sampling rate of your data should match the sampling rate of the data the model was pre-trained on
  - Total number of samples = Sampling rate * Duration of audio
4. Upsampling: Increasing the sampling rate of the signal
5. Downsampling: decreasing the sampling rate of the signal

# Nyquist Rate

1. Nyquist rate: Minimum rate at which a signal should be sampled to avoid aliasing, and it is twice the highest frequency present in the signal. If we sample below this rate --> aliasing. If we sample at or above this rate, we can perfectly reconstruct the original signal from the samples.
2. Aliasing: imagine a high-frequency waveform and its lower-frequency version. If we sampled a signal  at a lower rate, we might mistake the high-frequency waveform for its lower-frequency alias.

# Signal Reconstruction

1. Nyquist-Shannon sampling theorem: for perfect reconstruction fs>=2fm
2. Audio (fm = 20 KHz) ---> CD (sampling rate) = 44.1 KHz
3. fs<2fm --> aliasing might occur ---> causes different signals to become indistinguishable (or "aliases" of one another) when sampled.
4. The sampled audio signal is reconstructed back into a continuous (analog) form using digital-to-analog conversion (DAC)
5. DAC involves holding each sample value for its duration (a "zero-order hold") and then smoothing the resulting steps using a low-pass filter.

# Methods to Separate Components of the Audio

1. Frequency filtering: LPF, HPF, BPF
2. Fourier Transform: Decomposes a complex signal into its frequency components
3. Wavelet Transform: Decomposes a signal into both time and frequency components, making it useful for non-stationary signals.


# Fourier Transform



# Inverse Fourier Transform


# Audio Formats

**Uncompressed Format:**

1. WAV (Waveform Audio File Format): Uncompressed format (purely raw) ---> Developed by Microsoft and IBM ---> offers high-quality sound but take a lot of storage space

**Loss-less (Compression) Format:**

1. FLAC (Free Lossless Audio Codec): It can reduce file size without losing any quality, but it uses more storage space than lossy formats. 
2. ALAC (Apple Lossless Audio Codec): Apple's version of a lossless codec, providing similar quality to FLAC but in a format compatible with Apple devices.

**Lossy Format:**

1. MP3 (Moving Picture Experts Group (MPEG)): takes advantage of the human auditory system's limitations by removing audio data that is less perceptible to the average listener (generally too high or low freq + auditory masking: when louder sound masks or hides quieter sounds that occur simultaneously or in close proximity) allows for a significant reduction in file size while attempting to maintain the perceived audio quality.
2. AAC (Advanced Audio Coding): provides better sound quality than MP3 at the same bit rate. It's used by platforms like iTunes, YouTube, and many gaming systems.

# Librosa

1. Library for music and audio analysis
2. can handle any type of audio signal, but by default, it converts all signals to mono when loading an audio file because most audio analysis techniques do not require stereo
3. for stereo specify: mono=False when loading the audio
4. 

# Bit-depth

1. The bit depth of the sample determines with how much precision amplitude value can be described.
2. Higher bit-depth --> more info --> more resolution --> smaller quantization noise --> more faithfully the digital representation approximates the original continuous sound wave --> requires more data storage and processing power --> used for professional music production, film scoring, or high-definition video games
3. Lower bit-depth is preferred where storage is a constraint --> used for voice recordings, low-bandwidth streaming, or certain kinds of electronic music
4. The most common audio bit depths are 16-bit and 24-bit.
5. 16 bit-depth is enough --> quantization noise is small enough to be inaudible, and using higher bit depths is generally not necessary.
6. Dynamic Range (dB) = Bit Depth * 6.02
7. Quantization Noise: error between the original analog signal and the digital representation of that signal, introduced when the signal is digitized (quantized). ----> [quantizing involves rounding off the continuous value to a discrete value, the sampling process introduces noise]
8. The signal-to-quantization-noise ratio (SQNR) can be approximated with the formula: SQNR = 6.02N + 1.76 dB, where N is the bit depth. The higher the bit depth, the higher the SQNR, and the lower the impact of quantization noise.

# Spectrum

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/69ed77bd-75a6-40f0-a2bd-4118d4fbf255)

1. Representation of a signal in a frequency domain
2. amplitude Vs frequency
3. FT[time-domain signal]
4. shows the amplitude (or power) of each frequency component present in the signal.
5. does not provide any information about how these frequencies change over time.
6. helps in understanding its characteristics, such as bandwidth and the dominant frequencies.
7. 

# Spectrogram

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/4fef5eda-a899-4d95-b1c5-befadb32c1bc)

1. Visual Representation of the spectrum of frequencies as it varies with time -- a time-varying spectrum
2. FT[windowed excerpt of the signal]
3. Frequency, time, and amplitude
4. uses a linear frequency scale, which means all frequencies are equally spaced.
5. helps in identifying the frequency content of a signal over time, making it useful for speech analysis, music analysis, and even bird song identification.

# Mel-Spectrogram

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/d2fa0f32-5966-461f-95cd-54476156dd67)

1. uses the Mel frequency scale, which is a perceptual scale of pitches. The Mel scale better reflects the human ear's response to different frequencies, with higher resolution at lower frequencies and lower resolution at higher frequencies.

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/5fb9cadb-bc84-4535-b218-41331cca2292)


# Log-mel spectrogram

1. strength of the mel frequency components is represented in decibels ---> log-mel spectrogram
   
# MFCC

1. derived from the Mel-spectrogram by applying a Discrete Cosine Transform (DCT) to the log magnitudes of the Mel-spectrogram - gives a compressed representation of the Mel-spectrogram.
2. The DCT is a method for expressing a sequence of data points in terms of a sum of cosine functions oscillating at different frequencies. The DCT is similar to the Discrete Fourier Transform (DFT), but uses only real numbers and thus provides a more efficient representation for real-valued signals.
3. DCT helps to decorrelate the Mel-spectrum and yields a compressed representation of the spectral shape, which is the MFCCs.
4. Effectively captures the characteristics of the audio and is less sensitive to background noise

#
