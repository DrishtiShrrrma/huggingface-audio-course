# Transformer model:
1. introduced in the paper "Attention is All You Need" by Vaswani et al. in 2017. 
2. utilizes an attention mechanism that enables them to weigh the relevance of different elements in the input data.
3. An encoder in a Transformer model processes the input data and a decoder uses the output of the encoder to generate predictions.
4. Transformers use self-attention mechanisms to process all parts of the input data in parallel ---> Attention layers tell the model to pay specific attention to certain elements in the input sequence and ignore others when computing the feature representations. ---> This allows Transformers to handle long sequences more effectively and makes training faster and more efficient.


![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/5635a40e-3992-4fec-9189-17d94d5c7ab5)


# How Does a Transformer Work?
1. An input is passed to the model, which is then converted into embeddings.
2. These embeddings, along with positional encodings (to maintain sequence info), are passed through the encoder which generates a sequence of continuous representations using self-attention and a feed-forward network.
3. The encoder output is passed to the decoder, which also has self-attention and a feed-forward network, to generate predictions for the task.
4. This prediction (decoder's output) is then passed through a final linear layer and a softmax function to generate the final output probabilities.

# Components of a Transformer Model:
1. Input Embedding: The input text is first tokenized into subwords or words. Each token is then mapped to a vector via an embedding layer, resulting in a sequence of vectors that constitute the input embeddings.
2. Positional Encoding: Injects information about the position of the words in the sequence.
3. Encoder: Processes the input data in parallel using a self-attention mechanism and feed-forward neural network.
4. Multi-head attention: Multi-head attention allows the model to focus on different positions simultaneously, capturing various aspects of information from different positions. While a single head attention could capture some aspects of the information, using multiple heads allows the model to capture more complex relationships.
5. Add and Norm Block: The Add and Norm (or Residual Connection and Layer Normalization) block helps in stabilizing the hidden states and also allows the model to learn more complex functions. It is used after each sub-layer (self-attention and feed-forward) in both the encoder and the decoder.
6. Decoder: Uses the output from the encoder, and its self-attention mechanism, to generate predictions.
7. Output Linear Layer: Final layer of the decoder to generate the output.
8. Softmax Function: Normalizes the output of the Transformer, turning scores into probabilities.

# Transformer-based Models that take Waveform input

a. Wav2Vec, Wav2Vec-U, Wav2Vec2
b. HuBERT

1. a waveform is a one-dimensional sequence of floating-point numbers, where each number represents the sampled amplitude at a given time.
2. This raw waveform is first standardized (zero mean and variance =1)

# Wav2Vec

1. self-supervised pre-training + supervised fine-tuning
2. Similar to GPT-2 or GPT-3 for text, wav2vec tries to predict the future of an audio sequence
3. It’s a self-supervised model, meaning it trains itself by predicting future samples or contrastively distinguishing the true future context from other samples.
4. A large network on unlabeled data is pre-trained to learn useful contextual representations of the text/audio sequence.
5. There are however challenges to model the future of audio sequences compared to text. Audio data has a much higher dimension than text (text has a finite vocabulary), so directly modeling the waveform signal is much harder. Instead, wav2vec first reduces the dimensionality of the speech data by encoding it into a latent space, and then predicting the future in this latent space.
6. At the core of wav2vec are two distinct networks: the encoder network and the context network. Both are convolutional neural networks
7. The encoder network reduces the dimensionality of the speech data, by encoding 30 milliseconds of audio into a 512-dimensional feature vector zt at each timestep t, every 10 ms.
8. The context network takes as input the encoder output features, encoding 210 ms of raw audio into another 512-dimensional feature vector ct. The objective is to aggregate information over a longer timeframe to model higher-order information. This network outputs contextual representations ct that are used to predict future audio samples.
9. These convolution networks are causal in nature. Wav2vec should not “peek” into the future when predicting future samples, hence the convolutional layers are structured in a way that each output at time t never attends to positions after t. In practice, this is done through left-padding the input as shown in the diagram below.

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/48d6aa39-cd35-4f4a-bfb9-597f33e47b8f)

# Wav2Vec 2.0

1. A framework for self-supervised learning of speech representations that masks latent representations of the raw waveform and solves a contrastive task over quantized speech representations.

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/29bc1506-5fd0-44ae-87fc-9c3164c564d3)

2. Encodes speech audio via a multi-layer convolutional neural network and then masks spans of the resulting latent speech representations, similar to masked language
modeling.
3. The latent representations are fed to a Transformer network to build contextualized representations and the model is trained via a contrastive task where the true latent is to be distinguished from distractors.
4. As part of training, it learns discrete speech units via a gumbel softmax to represent the latent representations in the contrastive task which we find to be more effective than non-quantized targets.
5. After pre-training on unlabeled speech, the model is fine-tuned on labeled data with a Connectionist Temporal Classification (CTC) loss to be used for downstream speech recognition tasks.
6. Jointly learning discrete speech units with contextualized representations achieves substantially better results
7. Using only 10 minutes of labeled data, Wav2Vec2 achieves word error rate (WER) 4.8/8.2 on the clean/other test sets of Librispeech


# Wav2Vec-U

1. A method to train speech recognition models without any labeled data -- useful for ultra-low resource languages -- labelled data is sparse.
2. Wav2vec-U leverages self-supervised representations from wav2vec 2.0 to embed the speech audio and to segment the audio into units with a simple k-means clustering method. [It embeds and segments the speech audio with self-supervised representations from wav2vec 2.0, learns a mapping to phonemes with adversarial learning, and cross-validates hyper-parameter choices as well as early stopping with an unsupervised metric]
3. It learns a mapping between segments and phonemes using adversarial training and also labels segments as silences.
5. An unsupervised cross-validation metric is also introduced to enable model development without labeled development data.
6. It is very lightweight: consists of a single temporal convolution comprising only about 90k parameters to which we input frozen wav2vec 2.0 representations.
7. Compared to the best previous unsupervised work, wav2vec-U reduces the phoneme error rate on the TIMIT benchmark from 26.1 to 11.3. On the larger English Librispeech benchmark, wav2vec-U achieves a word error rate of 5.9 on test-other, rivaling some of the previous best-published systems trained on 960 hours of labeled data.
8. Silence Removal from the training data: rVAD, an unsupervised voice activity detection (VAD) model which determines the segments in the audio data corresponding to silences was applied and silent sections were removed.


![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/7f132d8a-5bdb-44eb-b0f6-c9f9ee02d433)

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/51fda51b-18c4-45a1-9cd6-f07fc73062c0)

8. Just like Wav2Vec, Wav2Vec-U is composed of a feature encoder and a context network. It also introduces a quantization module, a transformer model, and makes use of an online k-means algorithm for creating an unsupervised target.
9. It can still be computationally intensive due to the self-supervised learning nature.
10. Its use of online k-means allows for the creation of an unsupervised target, aiding in learning.
11. The performance may degrade for languages or domains significantly different from the training data.



# HuBERT (Hidden unit BERT)

1. Benefits from an offline clustering step to generate noisy labels for a BERT-like per-training. Concretely, a BERT model consumes masked continuous speech features to predict predetermined cluster assignments.
2. The predictive loss is only applied over the masked regions, forcing the model to learn good high-level representations of unmasked inputs to infer the targets of masked ones correctly.
3. HuBERT model is forced to learn both acoustic and language models from continuous inputs.
4. First, the model needs to model unmasked inputs into meaningful continuous latent representations, which maps to the classical acoustic modeling problem.
5. Second, to reduce the prediction error, the model needs to capture the long-range temporal relations between learned representations.
6. HuBERT benefits from the masked prediction loss over speech sequences to represent their sequential structure.
7. When the HuBERT model is pre-trained on either the standard Librispeech 960h or the Libri-Light 60k hours, it either matches or improves upon the state-of-the art wav2vec 2.0 performance on all fine-tuning subsets of 10mins, 1h, 10h, 100h, and 960h.
8. The X-LARGE model shows up to 19% and 13% relative WER improvement from LARGE models on dev-other and test-other evaluation subsets when pre-trained on the Libri-Light 60k hours.

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/f420114b-3a3d-4850-8a6b-1252f349860d)

