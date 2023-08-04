# Sequence-to-Sequence Architectures

1. Encoder-decoder model (seq2seq archi)
2. model maps a sequence of one kind of data to a sequence of another kind of data.
3. These models don't have a one-to-one correspondence between input and output sequences, allowing for different lengths of input and output. They're useful for tasks such as text summarization, translation between different languages, and audio tasks like speech recognition.
4. the architecture of the decoder is similar to the encoder, both making use of layers with self-attention as a crucial feature. However, the decoder's task differs from the encoder's. The decoder generates the output sequence based on the understanding developed by the encoder from the input sequence.

# Encoder-only Models

1. make a prediction for each element in the input sequence. This leads to an equal length of input and output sequences. For audio inputs, these models often downsample the waveform before making a prediction for every designated segment of audio (like every 20 ms).
2. ctc-based models
