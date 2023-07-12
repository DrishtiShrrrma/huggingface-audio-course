# Transformer model:
1. introduced in the paper "Attention is All You Need" by Vaswani et al. in 2017.
2. utilizes an attention mechanism that enables them to weigh the relevance of different elements in the input data.
3. An encoder in a Transformer model processes the input data and a decoder uses the output of the encoder to generate predictions.
4. Transformers use self-attention mechanisms to process all parts of the input data in parallel.


# How Does a Transformer Work?
1. An input is passed to the model, which is then converted into embeddings.
2. These embeddings, along with positional encodings (to maintain sequence info), are passed through the encoder which generates a sequence of continuous representations using self-attention and a feed-forward network.
3. The encoder output is passed to the decoder, which also has self-attention and a feed-forward network, to generate predictions for the task.
4. This prediction (decoder's output) is then passed through a final linear layer and a softmax function to generate the final output probabilities.

# Component of Transformers Model:
1. Input Embedding:
2. Positional Encoding: Injects information about the position of the words in the sequence.
3. Encoder: Processes the input data in parallel using a self-attention mechanism and feed-forward neural network.
4. Decoder: Uses the output from the encoder, and its self-attention mechanism, to generate predictions.
5. Output Linear Layer: Final layer of the decoder to generate the output.
6. Softmax Function: Normalizes the output of the Transformer, turning scores into probabilities.