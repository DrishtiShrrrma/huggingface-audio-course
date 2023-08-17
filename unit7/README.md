# Speech-to-Speech Translation ((STST or S2ST)

1. speech (language 1)  ---> speech (language 2)
2. Application: multilingual communication

![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/61068b4d-e0ee-4801-b362-10ef2c45ca3b)

# Cascaded Approach to STST

1. Cascaded Approach: Uses a speech translation (ST) system to directly transcribe the source speech into text in the target language, then applies text-to-speech (TTS) to generate speech in the target language - **ST (speech translation) + TTS (text-to-speech)** - compute and data efficient - also suffers from the issues of error propagation and additive latency (but relatively less)
2. Three-Stage Approach: Involves three distinct stages: first, an automatic speech recognition (ASR) system transcribes the source speech into text in the same language; next, machine translation (MT) translates the transcribed text into the target language; finally, text-to-speech (TTS) generates speech in the target language - **ASR (automatic speech recognition), MT (machine translation), and TTS (text-to-speech)** - may lead to error propagation, where errors in one stage are compounded in subsequent stages - also increases latency since inference must be conducted for more models.
3. 


![image](https://github.com/DrishtiShrrrma/huggingface-audio-course/assets/129742046/846d5be7-20ef-4183-b15e-6e88075916cf)


# Direct Approach to STST

1. does not predict an intermediate text output and instead maps directly from source speech to target speech.
2. also capable of retaining the speaking characteristics of the source speaker in the target speech (such a prosody, pitch and intonation).
