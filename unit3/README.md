# Transformer model:
1. introduced in the paper "Attention is All You Need" by Vaswani et al. in 2017.
2. utilizes an attention mechanism that enables them to weigh the relevance of different elements in the input data.
3. An encoder in a Transformer model processes the input data and a decoder uses the output of the encoder to generate predictions.
4. Transformers use self-attention mechanisms to process all parts of the input data in parallel ---> This allows Transformers to handle long sequences more effectively and makes training faster and more efficient.

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/847682e0-4589-45bc-96f2-7bf24a3c4021)

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
