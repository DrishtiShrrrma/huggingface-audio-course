# Spectrograms Vs Normal Images

1. Shifting the contents of an image vertically up or down generally doesn't alter its meaning, but in the case of a spectrogram, vertical shifts change the frequencies and, subsequently, the audio's character.
2. Images - translation-invariant, spectrograms are not invariant to shifts.

# Audio Classification Architectures

1. Goal is to predict a class label for an audio input
2. label can correspond to an entire audio chunk or audio segment(s)
3. spectrogram - 2d tensor (freqs, seq len)


### Audio Spectrogram Transformer

1. uses the ViT or Vision Transformer model and takes spectrograms as input instead of regular images for audio classification tasks.
2. The transformer's self-attention layers in AST help the model capture global context more effectively than traditional CNNs.
3. Similar to ViT, AST divides the audio spectrogram into a sequence of partially overlapping image patches, typically of size 16×16 pixels.
4. The sequence of patches is then projected into a sequence of embeddings and fed into the transformer encoder as input.
5. AST is an encoder-only transformer model, and its output is a sequence of hidden-states, where each hidden-state corresponds to a 16×16 input patch.
6. A simple classification layer with sigmoid activation is applied on top of the hidden-states to map them to classification probabilities, producing the final output for audio classification

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/ff6162b0-47ca-46e0-ad92-f991a2c4c44b)



