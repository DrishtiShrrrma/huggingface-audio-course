
# Instructions

- Fine-tune the ”openai/whisper-tiny” model using the American English (“en-US”) subset of the ”PolyAI/minds14” dataset.
- Use the first 450 examples for training, and the rest for evaluation. Ensure you set num_proc=1 when pre-processing the dataset using the .map method (this will ensure your model is submitted correctly for assessment).
- To evaluate the model, use the wer and wer_ortho metrics as described in this Unit. However, do not convert the metric into percentages by multiplying by 100 (E.g. if WER is 42%, we’ll expect to see the value of 0.42 in this exercise).
