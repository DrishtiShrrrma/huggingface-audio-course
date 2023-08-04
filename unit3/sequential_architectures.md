# Spectrogram

1. spectrogram is made by taking the frequency spectrum of successive time slices of an audio waveform and stacking them together

# Sequence-to-Sequence Architectures

1. Encoder-decoder model (seq2seq archi)
2. model maps a sequence of one kind of data to a sequence of another kind of data.
3. These models don't have a one-to-one correspondence between input and output sequences, allowing for different lengths of input and output. They're useful for tasks such as text summarization, translation between different languages, and audio tasks like speech recognition.
4. the architecture of the decoder is similar to the encoder, both making use of layers with self-attention as a crucial feature. However, the decoder's task differs from the encoder's. The decoder generates the output sequence based on the understanding developed by the encoder from the input sequence.
5. It's common for these seq2seq models to use spectrograms as input. However, a seq2seq model can also be designed to work directly on audio waveforms.

# Encoder-only Models

1. make a prediction for each element in the input sequence. This leads to an equal length of input and output sequences. For audio inputs, these models often downsample the waveform before making a prediction for every designated segment of audio (like every 20 ms).
2. ctc-based models

# ASR (Seq2Seq Model)

### Whisper

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/3015f2bb-60bd-473c-8b39-a1161cc29829)

1. Encoder takes log-mel as input and encodes that spectrogram to form a sequence of encoder hidden states that extract important features from the spoken speech.
2. hidden-states tensor represents the input sequence as a whole and effectively encodes the “meaning” of the input speech.
3. The output of the encoder is then passed into the transformer decoder using cross-attention. This is like self-attention but attends over the encoder output.
4. The decoder predicts a sequence of text tokens in an autoregressive manner, a single token at a time, starting from an initial sequence that just has a “start” token in it (SOT in the case of Whisper).
5. At each following timestep, the previous output sequence is fed back into the decoder as the new input sequence. In this manner, the decoder emits one new token at a time, steadily growing the output sequence, until it predicts an “end” token or a maximum number of timesteps is reached.
6. the decoder has a cross-attention mechanism (which is causal - can't look into the future) that allows it to look at the encoder’s representation of the input sequence
7. the decoder plays the role of a language model, processing the hidden-state representations from the encoder and generating the corresponding text transcriptions - better approach - gives greater flexibility and generally superior performance.
8. CTC model outputs a sequence of individual characters, the tokens predicted by Whisper are full words or portions of words.
9. uses the tokenizer from GPT-2 and has 50k+ unique tokens- a seq2seq model can therefore output a much shorter sequence than a CTC model for the same transcription.
10. loss function for a seq2seq ASR model is the cross-entropy loss, as the final layer of the model predicts a probability distribution over the possible tokens. This is usually combined with techniques such as beam search to generate the final sequence.
11. The metric for speech recognition is WER or word error rate, which measures how many substitutions, insertions, and deletions are necessary to turn the predicted text into the target text — the fewer, the better the score.
12. 

# Text-to-Speech

1. encoder takes in a sequence of text tokens and extracts a sequence of hidden-states that represent the input text.
2. The transformer decoder applies cross-attention to the encoder output and predicts a spectrogram.
3. For the TTS model, we start by the decoding with a spectrogram of length one that is all zeros that acts as the “start token”.
4. Given this initial spectrogram and the cross-attentions over the encoder’s hidden-state representations, the decoder then predicts the next timeslice for this spectrogram, steadily growing the spectrogram one timestep at a time.
5. how does the decoder know when to stop? In the SpeechT5 model this is handled by making the decoder predict a second sequence. This contains the probability that the current timestep is the last one.  While generating audio at inference time, if this probability is over a certain threshold (say 0.5), the decoder is indicating that the spectrogram is finished and the generation loop should end.
6. After decoding, SpeechT5 utilizes a post-net consisting of convolution layers to refine the spectrogram.
7. During training, the TTS model uses spectrograms as targets and employs L1 or MSE loss.
8. To convert the output spectrogram into an audio waveform, an external model called the vocoder is used, which is trained separately from the seq2seq architecture.
9. TTS is challenging due to the one-to-many mapping between input text and possible speech sounds, making it hard to evaluate with traditional loss functions - Human listeners' evaluations using MOS or mean opinion score are often used to evaluate TTS models.



![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/6c48f846-c087-4ed0-a57a-28c2af0fa43c)
