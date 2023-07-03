# Audio/Sound
1. Real-world signal ---> analog or continuous
2. Audio can be stored in analog + digital format
3. Analog Storage Medium: Vinyl records or cassette tapes
4. In most modern systems, audio is converted from analog ---> digital (ADC): for ease of storage, transmission, and manipulation (digital processing)


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

1. Sampling:
2. Upsampling: Increasing the sampling rate of the signal
3. Downsampling: decreasing the sampling rate of the signal

# Nyquist Rate

1. Nyquist rate: Minimum rate at which a signal should be sampled to avoid aliasing, and it is twice the highest frequency present in the signal. If we sample below this rate --> aliasing. If we sample at or above this rate, we can perfectly reconstruct the original signal from the samples.

# Signal Reconstruction

1. Nyquist-Shannon sampling theorem: for perfect reconstruction fs>=2fm
2. Audio (fm = 20 Khz) ---> CD (sampling rate) = 44.1 KHz
3. fs<2fm --> aliasing might occur ---> causes different signals to become indistinguishable (or "aliases" of one another) when sampled.
4. The sampled audio signal is reconstructed back into a continuous (analog) form using digital-to-analog conversion (DAC)
5. DAC involves holding each sample value for its duration (a "zero-order hold") and then smoothing the resulting steps using a low-pass filter.