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


# ASR: CTC OR Sequence-to-Sequence Models

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


# Note

1. Multi-lingual checkpoints are heavier
2. 

# Advantages of Whisper Model

1. handles long-form audio samples, its robustness to input noise and ability to predict cased and punctuated transcriptions.
2. Whisper is pre-trained on a vast quantity of labelled audio-transcription data, 680,000 hours to be precise - better results
3. 

# Features that are Essential to Consider when Choosing a Speech Recognition Dataset

1. **Number of Hours**

- Size of the dataset.
- we want a **diverse** dataset with lots of different speakers, domains and speaking styles.

2. **Domain**

- Source of Data: The domain refers to where the data was sourced from, such as audiobooks, podcasts, YouTube, or financial meetings.

- Distribution of Data: Each domain has a unique distribution of data, reflecting different characteristics and qualities.

- Audio Quality: Consideration of audio quality and style (e.g., background noise, formal vs. informal speech). For example:

    - Audiobooks: Typically recorded in high-quality studio conditions with no background noise.
    - YouTube: Likely to contain more background noise and a more informal style of speech.
- Matching Domain to Inference Conditions: It's essential to select a domain that aligns with the conditions anticipated at inference time. Training on a domain that doesn't match the intended use can lead to poor performance. For example, a model trained on audiobooks may not perform well in noisy environments.

3. **Speaking Style**

- Narrated (read from a script) vs. Spontaneous (un-scripted, conversational speech).
- Differences in articulation, errors, colloquialisms, repetitions, hesitations, and false-starts.

4. **Transcription Style**

- Presence or absence of punctuation and casing.
- no need for punc or casing - if only the spoken words are needed without specific formatting
- punc + casing - if the goal is to generate fully formatted text for purposes like publication or meeting transcription
- Requirements for fully formatted text vs. unformatted structure.
- Depending on the requirements, one can either choose a dataset without punctuation or casing or select one that includes these features and then remove them through pre-processing.


# Evaluation Metrics for ASR

### 1. Word Error Rate

1. calculates substitutions, insertions and deletions on the **word level** - errors are annotated on a word-by-word basis
2. can be higher in noisy env

3. 

#### Limitations of WER

1. treats all errors (substitutions, insertions, deletions) equally, regardless of their impact on the meaning of the transcribed text (An error in a critical word may have more significant consequences than an error in a less important word)
2. doesn't consider the semantic importance of words. A mistake in a key term might change the entire meaning of a sentence
3. doesn't take into account the context in which words are used. Errors that might be considered minor in casual conversation could be more serious in a formal or technical context.
4. no consideration of grammar
5. calculation of WER depends on the reference transcription, and different references might lead to different WER values.
6. WER might not be equally suitable for all languages, especially those with complex morphology or those that rely heavily on context and word order.
7. WER may not adequately capture errors related to accents and dialects, leading to biased evaluations if the system is tested on speech that differs from the training data




