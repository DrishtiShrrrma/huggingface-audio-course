# Misc

1. Encoder-only models are preferred for audio classification due to their simplicity and faster inference speed.
2. Decoder-only models add unnecessary complexity to the task as they generate multiple outputs, assuming the outputs can be a sequence of predictions instead of a single class label prediction
3. The architectural choices for audio classification models mirror those in natural language processing (NLP), where encoder-only models like BERT are favored for sequence classification tasks, while decoder-only models like GPT are reserved for sequence generation tasks.

# Keyword Spotting

1. objective is to determine whether the given audio contains the targeted keyword or not
2. Datasets: [MINDS-14](https://huggingface.co/datasets/PolyAI/minds14), [Speech Commands](https://huggingface.co/datasets/speech_commands), 

### TO-DO

1. Fine-tune wav2vec2 - keyword spotting task
2. fine-tune ASR on Speech Commands dataset

# Language Identification (LID)

1. Objective is to identify the language spoken in an audio sample from a list of candidate languages.
2. an LID model can be used to categorise the language(s) spoken in the audio sample, and then select an appropriate speech recognition model trained on that language to transcribe the audio.
3. Datasets: [FLEURS](https://huggingface.co/datasets/google/fleurs)

### TO-DO

1. fine-tune Whisper on FLEURS dataset (currently the most performant model is "sanchit-gandhi/whisper-medium-fleurs-lang-id")






