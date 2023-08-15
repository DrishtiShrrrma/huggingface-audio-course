# ASR: Challenges

1. Understanding both audio and text.
2. Interference from extraneous sounds can make recognition difficult.
3. Variability in accents can complicate understanding.
4. Characters in written text that don’t have an acoustic sound (e.g., punctuation) are challenging to infer from audio.
5. Differences in pitch, tone, and speech speed among individuals.
6. Challenges arising from different languages and dialects.
7. Difficulty in distinguishing words that sound the same but have different meanings or spellings.
8. Overlapping adjacent phonemes make them hard to separate and recognize.
9. Reflection of sound off surfaces can distort the signal.
10. Recognizing speech from multiple speakers talking simultaneously.
11. Altered pronunciation due to emotional state or stress.
12. Hindrance due to poor quality or low-resolution audio.
13. Inaccurate recognition of specialized vocabulary or jargon.
14. Challenges with speech that is spoken too quickly or without clear enunciation.


# ASR : CTC OR Sequence-to-Sequence Models

1. CTC: encoder-only models with a linear classification (CTC) head on top
- pre-trained checkpoint could be fine-tuned with a CTC head on as little as 10 minutes of labelled speech data to achieve strong performance on a downstream speech recognition task.
- Shortcomings: Appending a simple linear layer to an encoder gives a small, fast overall model, but can be prone to phonetic spelling errors. 
3. Seq2Seq: encoder-decoder models, with a cross-attention mechanism between the encoder and decoder


# I) CTC

1. A CTC model is essentially an 'acoustic-only' model.
2. consists of an encoder forming hidden-state representations from audio inputs.
3. A linear layer maps the hidden-states to characters.
4. The system bases its predictions almost entirely on the acoustic input (phonetic sounds of the audio).
5. There is a tendency to transcribe the audio in a phonetic way (e.g., CHRISTMAUS).
6. The model gives less importance to the language modeling context of previous and successive letters.
7. The model is prone to phonetic spelling errors.
8. An intelligent model would recognize and correct invalid words in the vocabulary (e.g., CHRISTMAS instead of CHRISTMAUS).
9. The model's prediction lacks two big features - casing and punctuation.
10. The absence of casing and punctuation limits the usefulness of the model's transcriptions in real-world applications.

# II) Seq2Seq

1. encoder and decoder linked via a cross-attention mechanism.
2. encoder --> computes hidden-state representations of the audio inputs + decoder --> language model.
3. The decoder processes the entire sequence of hidden-state representations from the encoder and generates the corresponding text transcriptions.
4. With global context of the audio input, the decoder is able to use language modelling context as it makes its predictions, correcting for spelling mistakes on-the-fly and thus circumventing the issue of phonetic predictions.
5. Unlike its CTC predecessors, which were pre-trained entirely on un-labelled audio data, Whisper is pre-trained on a vast quantity of labelled audio-transcription data, 680,000 hours to be precise.
6. 



# Downsides to Seq2Seq


1. inherently slower at decoding, since the decoding process happens one step at a time, rather than all at once.
2. more data hungry --> requires significantly more training data to reach convergence ---> **labelled** speech data ---> scarce
3. 
