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
