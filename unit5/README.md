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

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/c6f4d173-db22-4da5-bb6b-9e99989a45b2)

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/4833c042-2134-4b9b-a964-94bc95a2d033)



#### Limitations of WER

1. treats all errors (substitutions, insertions, deletions) equally, regardless of their impact on the meaning of the transcribed text (An error in a critical word may have more significant consequences than an error in a less important word)
2. doesn't consider the semantic importance of words. A mistake in a key term might change the entire meaning of a sentence
3. doesn't take into account the context in which words are used. Errors that might be considered minor in casual conversation could be more serious in a formal or technical context.
4. no consideration of grammar
5. calculation of WER depends on the reference transcription, and different references might lead to different WER values.
6. WER might not be equally suitable for all languages, especially those with complex morphology or those that rely heavily on context and word order.
7. WER may not adequately capture errors related to accents and dialects, leading to biased evaluations if the system is tested on speech that differs from the training data
8. In cases of very short utterances, a single error can lead to a very high WER, making it a less reliable metric for evaluating performance on short segments.
9. can be affected by the rate of speech, and models might perform differently on slow versus fast speech, even if the content is the same.
10. doesn't give detailed insights into specific types of mistakes, such as misrecognizing certain phonemes or consistently struggling with certain grammatical structures.


### 2. Word Accuracy

1. also measured on the word-level, it’s just the WER reformulated as an accuracy metric rather than an error metric.
2. very infrequently quoted in the speech literature
3. 

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/49e95a9a-68e0-4201-b11c-50a3541723d6)

### 3. Character Error Rate

1. assesses on character-level - words are divided into their individual characters, and errors are annotated on a character-by-character basis
2. provides a more detailed analysis at the character level, making it sensitive to small errors that might be overlooked by WER.
3. more suitable for languages where word segmentation is challenging or ambiguous.
4. Less Sensitive to Reference Transcription


![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/7546a752-be22-46df-948c-87f54ef75219)



![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/892e4a3a-438b-49c3-a704-980fa401bf2b)

Note: CER + WER both metrics in conjunction can provide a more comprehensive evaluation of a speech recognition system.

# Which Metrics Should I Use?

1. WER is usually preferred over CER --> context-aware
2. WER is less forgiving than CER but is more conducive to developing intelligible speech systems that understand language nuances.
3. for languages like Mandarin and Japanese --> CER is preferred
4. WER is computed over an entire test set consisting of several thousand sentences, not just one.
5. When evaluating over multiple sentences, the substitutions (S), insertions (I), deletions (D), and total words (N) are aggregated across all sentences before computing the WER.

# Normalization

1. Normalising the dataset leads to lower WER, indicating better results. However, this doesn't necessarily mean the model is better for production use, as the lack of formatting makes the text harder to read.
2. if ASR model is trained on data (casing + punc) ---> it will learn to predict these elements in its transcriptions.  ---> results in fully formatted text (orthographic style) ---> suitable for meeting transcription or dictation
3. Wav2Vec2 does not predict punctuation or casing, while Whisper does, making the Whisper transcription easier to read - with wav2vec2, LM is required
4. While normalisation can improve WER, it may come at the cost of readability and applicability in real-world scenarios where formatted text is desired.


5. There is an option to normalise the dataset by removing casing and punctuation.

