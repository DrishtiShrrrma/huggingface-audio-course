1. **Choose the Application:** Select the cascaded speech-to-speech translation Gradio demo from the first section of the Unit.

2. **Duplicate the Template:** Start by duplicating the template under your Hugging Face namespace.

3. **Set Up the Environment:** Ensure that you can run the demo without a GPU accelerator device, using the free CPU tier.

4. **Set Visibility:** Make sure the visibility of your demo is set to public so that it can be accessed and checked for correctness.

5. **Update the Speech Translation Function:**

- Modify the function to perform multilingual speech translation.
- Follow the instructions provided in the section on speech-to-speech translation to translate from speech in language X to text in language Y.
Synthesize Text to Speech:

6. **Use a multilingual TTS checkpoint to synthesize from text in language Y to speech in language Y**
- Choose between the SpeechT5 TTS checkpoint that you fine-tuned previously or a pre-trained multilingual TTS checkpoint.
- Consider using the Dutch split of the VoxPopuli dataset or an MMS TTS checkpoint, depending on your language preference.
- Update Requirements: If using an MMS TTS checkpoint, update the requirements.txt file to install transformers from the specified PR branch.

7. **Design the Demo:**

- The demo should accept an audio file as input and return another audio file as output.
- Maintain the main function speech_to_speech_translation and update the translate and synthesize functions as needed.
- Build the Gradio Demo: Create the demo as a Gradio demo on the Hugging Face Hub.

8. **Submit for Assessment:**

- Head to the specified Space and provide the repository ID of your demo.
- The Space will check if the returned audio file is non-English.
- If successful, you'll receive a green tick next to your name on the overall progress space.
- Consider Performance: Experiment with different checkpoints and languages to find the best performance for your specific use case.

9. **Follow Tips and Guidelines:** Refer to the provided tips and guidelines for additional assistance in updating the speech translation function and other aspects of the demo.
