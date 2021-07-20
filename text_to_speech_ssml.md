```python
!pip install --upgrade google-cloud-texttospeech
```


```python
import html

from google.cloud import texttospeech


```


```python
#print("hi")

def ssml_to_audio(ssml_text, outfile):
    # Generates SSML text from plaintext.
    #
    # Given a string of SSML text and an output file name, this function
    # calls the Text-to-Speech API. The API returns a synthetic audio
    # version of the text, formatted according to the SSML commands. This
    # function saves the synthetic audio to the designated output file.
    #
    # Args:
    # ssml_text: string of SSML text
    # outfile: string name of file under which to save audio output
    #
    # Returns:
    # nothing

    # Instantiates a client
    client = texttospeech.TextToSpeechClient()

    # Sets the text input to be synthesized
    synthesis_input = texttospeech.SynthesisInput(ssml=ssml_text)

    # Builds the voice request, selects the language code ("en-US") and
    # the SSML voice gender ("MALE")
    voice = texttospeech.VoiceSelectionParams(
        language_code="en-US", ssml_gender=texttospeech.SsmlVoiceGender.MALE
    )

    # Selects the type of audio file to return
    audio_config = texttospeech.AudioConfig(
        audio_encoding=texttospeech.AudioEncoding.MP3
    )

    # Performs the text-to-speech request on the text input with the selected
    # voice parameters and audio file type
    response = client.synthesize_speech(
        input=synthesis_input, voice=voice, audio_config=audio_config
    )

    # Writes the synthetic audio to the output file.
    with open('outfile.mp3', "wb") as out:
        out.write(response.audio_content)
        print("Audio content written to file " + outfile)
        
ssml_to_audio("""Hello....""", 'audio')
```

    Audio content written to file audio



```python
Text="If you know.<break strength="weak"/>.<break strength="weak"/> your party's 3 digit extension.<break strength="weak"/>.<break strength="weak"/>  you may dial it at any time.<break strength="weak"/>.<break strength="weak"/>  "
SSMLText = binary:replace(Text,<<".">>,<<".<break strength=\"weak\"/>">>,[global]),
SSMLText2 = binary:replace(SSMLText,<<",">>,<<".<break strength=\"weak\"/>">>,[global]),
ssml_text = """<<"<speak>",SSMLText2/binary,"</speak>">>"""

```
