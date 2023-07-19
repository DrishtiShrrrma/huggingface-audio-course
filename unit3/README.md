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

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/ee8f3aff-289f-4753-8b08-89e38cef8684)

# Wav2Vec 2.0

1. A framework for self-supervised learning of speech representations that masks latent representations of the raw waveform and solves a contrastive task over quantized speech representations.
2. 

# Wav2Vec-U

1. A method to train speech recognition models without any labeled data -- useful for ultra-low resource languages -- labelled data is sparse.
2. Wav2vec-U leverages self-supervised representations from wav2vec 2.0 to embed the speech audio and to segment the audio into units with a simple k-means clustering method. [It embeds and segments the speech audio with self-supervised representations from wav2vec 2.0, learns a mapping to phonemes with adversarial learning, and cross-validates hyper-parameter choices as well as early stopping with an unsupervised metric]
3. It learns a mapping between segments and phonemes using adversarial training and also labels segments as silences.
5. An unsupervised cross-validation metric is also introduced to enable model development without labeled development data.
6. It is very lightweight: consists of a single temporal convolution comprising only about 90k parameters to which we input frozen wav2vec 2.0 representations.
7. Compared to the best previous unsupervised work, wav2vec-U reduces the phoneme error rate on the TIMIT benchmark from 26.1 to 11.3. On the larger English Librispeech benchmark, wav2vec-U achieves a word error rate of 5.9 on test-other, rivaling some of the previous best-published systems trained on 960 hours of labeled data.
8. Silence Removal from the training data: rVAD, an unsupervised voice activity detection (VAD) model which determines the segments in the audio data corresponding to silences was applied and silent sections were removed.
9. 


![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/7f132d8a-5bdb-44eb-b0f6-c9f9ee02d433)

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/51fda51b-18c4-45a1-9cd6-f07fc73062c0)

7.  self-supervised speech representations are leveraged to segment unlabeled audio and learn a mapping from these representations to phonemes via adversarial training. The right representations are key to the success of our method.
8. Just like Wav2Vec, Wav2Vec-U is composed of a feature encoder and a context network. It also introduces a quantization module, a transformer model, and makes use of an online k-means algorithm for creating an unsupervised target.
9. It can still be computationally intensive due to the self-supervised learning nature.
10. Its use of online k-means allows for the creation of an unsupervised target, aiding in learning.
11. The performance may degrade for languages or domains significantly different from the training data.



# HuBERT


