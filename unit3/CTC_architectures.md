# CTC:

1. Connectionist Temporal Classification (CTC) is a type of loss function used in neural networks, particularly for sequence-to-sequence problems where the alignment between the input and output sequences is unknown.
2. Primarily designed for tasks like speech recognition, handwriting recognition, etc.
3. **While CTC is commonly used with encoder-only transformer models in automatic speech recognition (ASR), it is not exclusive to this setup.**
4. An encoder-only ASR model works by transforming an input audio waveform into a sequence of feature vectors, then applying a transformation (the encoder) to map these feature vectors into a sequence of hidden states. The decoder, in a traditional sense, is not needed because CTC can directly map from the hidden states to the output sequence of labels. The CTC "decoding" process involves finding the most likely label sequence given the model's output probabilities.
5. The blank token in CTC is a special symbol that the model can output when it is uncertain, or when there is no character to output. It helps the model handle variable-length input-output mappings, and also assists in handling character duplication. The blank symbol is typically used in the decoding process to remove duplicates and to align the predicted sequence with the target sequence.
6. **We know that the order the speech is spoken in is the same as the order that the text is transcribed in (the alignment is so-called monotonic), but we don’t know how the characters in the transcription line up to the audio. This is where the CTC algorithm comes in.**
7. During decoding, repeated characters (not separated by a blank symbol) are collapsed into a single character.**
8. if the raw output is 'a-b-a-b', where '-' represents the blank symbol, it would be decoded as 'abab', preserving the actual repeated characters. Therefore, the process distinguishes between truly repeated characters and artifacts of the model's forced output at each timestep.   
9. An ASR model is trained on a dataset consisting of (audio, text) pairs where the text is a human-made transcription of the audio file. Generally the dataset does not include any timing information that says which word or syllable occurs where in the audio file. Since we can’t rely on timing information during training, we don’t have any idea how the input and output sequences should be aligned.
10. Use: Solving sequence-to-sequence problems where the alignment between the input and output sequences is unknown, ASR, etc!
11. vocab is usually <=50, either only uppercase or lower case is used + numbers are spelled out (20 --> twenty)

# Advantages of CTC:

1. No need for alignment between input and output sequences
2. Efficient in handling sequences of varying lengths
3. When the input sequences and output sequences are of comparable length.
4. In tasks with a relatively small number of output classes, such as character-level automatic speech recognition.
5. Can handle many sequence-to-sequence tasks quite effectively

# Disadvantages of CTC:

1. Makes the assumption that the inputs are independent given the outputs, which might not hold true for all types of data
2. Doesn't work well for problems where there are strong dependencies between output labels - conditional independence assumption of CTC is often violated in such cases.
3. In cases where the input sequence and output sequence have significantly different lengths.
4. It can struggle with particularly long sequences due to its need to assign an output (or blank label) to every input timestep.
5. If the number of output classes is large, CTC may not perform well. It is more suited to tasks with a smaller number of output classes.
6. It may output words that sound correct, but are not spelled correctly, since it only considers individual characters, not complete words. This can be improved by using an external language model which acts as a spellchecker on top of the CTC output.

# CTC Head

1.  A CTC head is a part of the model that takes the hidden states output by the encoder and produces a sequence of probabilities for each possible output label at each time step.
2.  This is usually implemented as a fully-connected layer that maps from the size of the hidden states to the number of possible output labels.
3.  The output of a CTC head is indeed a sequence of probabilities for each possible output label at each time step.



# Hidden States

1. Intermediate outputs of the model. They can be either high-dimensional or low-dimensional, depending on the specific architecture of the model.

# CNN Feature Encoder

1. Used to extract feature vectors from an input.
2. For an audio signal, it might take as input a spectrogram or other time-frequency representation of the audio and output a sequence of feature vectors.
3. The specific architecture of the CNN can vary, but it often includes several layers of convolution, pooling, and non-linear activation functions.

# ASR Output Prediction

1. In ASR, the speech is converted to a text output.
2. The way this text is predicted depends on the approach used. It can be as individual characters (each label in the output sequence is a character), as phonemes (each label is a phoneme, the smallest unit of sound), or as word tokens (each label is a whole word). The choice depends on the specific task and the data available.



# Wav2Vec

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/ff676ed4-de97-462c-bd00-d36ae0308274)
