# Azure-speech-service

## 1. Prerequisites
Before you can use Azure Speech Services, you need to have the following:

* An Azure subscription
* Python 3.5 or later
* A Speech Services API key
* A Speech Services region
You can get an API key and set up a Speech Services region by following the instructions on the Azure portal. Once you have these prerequisites, you can install the Azure SDK for Python using pip:
```
pip install azure-cognitiveservices-speech
```
When setting up the Speech SDK configuration, you should specify the appropriate service region based on the location closest to your application and data. This can help reduce latency and improve the overall performance of your application:
```
westus
westus2
eastus
eastus2
centralus
northcentralus
southcentralus
westeurope
northeurope
southeastasia
eastasia
australiaeast
brazilsouth
canadacentral
francecentral
koreacentral
japaneast
japanwest
uksouth
westus3
southafricanorth
uaenorth
```
## 2. Voice to text
The Azure Speech Services can be used to convert spoken language to text. This is useful for building applications that can transcribe speech in real-time. Here is how you can use the Speech SDK to convert spoken language to text:
```python
import azure.cognitiveservices.speech as speechsdk

# Set up the Speech SDK configuration
speech_key = "YourSpeechServicesApiKey"
service_region = "YourServiceRegion"
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)

# Set up the audio configuration
audio_config = speechsdk.AudioConfig(use_default_microphone=True)

# Set up the speech recognizer
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_config)

# Recognize speech
print("Say something...")
result = speech_recognizer.recognize_once()
print("You said:", result.text)
```
This code sets up the Speech SDK configuration and audio configuration, creates a SpeechRecognizer object, and then calls the `recognize_once` method to transcribe the spoken language to text. The `use_default_microphone` parameter in the `AudioConfig` constructor specifies that the Speech SDK should use the default microphone on the device. To use a different microphone use de `device_name`:
```python
audio_config = speechsdk.AudioConfig(device_name="Microphone (2- Audio-Technica USB mic...")
```
You can find the name of your microphone or audio input device by using the `list_microphones` method of the `AudioConfig` class. This method returns a list of available microphones and their device names. You can then select the appropriate device name for your use case.

## 3. Text to voice
The Azure Speech Services can also be used to convert text to spoken language. This is useful for building applications that can generate speech in real-time. Here is how you can use the Speech SDK to convert text to spoken language:
```python
import azure.cognitiveservices.speech as speechsdk

# Set up the Speech SDK configuration
speech_key = "YourSpeechServicesApiKey"
service_region = "YourServiceRegion"
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)

# Set the language to English (US) and the voice to "Microsoft Server Speech Text to Speech Voice (en-US, Guy24KRUS)"
speech_config.speech_synthesis_language = "en-US"
speech_config.speech_synthesis_voice_name = "Microsoft Server Speech Text to Speech Voice (en-US, Guy24KRUS)"

# Set the voice, style, rate, and pitch
speech_config.speech_synthesis_style = speechsdk.SpeechSynthesisStyle.chat
speech_config.speech_synthesis_rate = 0.8
speech_config.speech_synthesis_pitch = -1.0

# Set up the speech synthesizer
speech_synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config)

# Synthesize speech
result = speech_synthesizer.speak_text_async("How are you doing?").get()

# Play audio
audio_player = speechsdk.AudioPlayer()
audio_player.play_audio(result.audio_data)
```
In this example, the speech_synthesis_language property is set to "en-US" to specify that the text should be synthesized in English. You can find a list of supported language codes in the [Azure Text-to-Speech documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/language-support). To get the list of available voices for a given language, you can use the `speechsdk.SpeechSynthesisVoice.get_all_voices` method. Here's an example:
```python
voices = speechsdk.SpeechSynthesisVoice.get_all_voices("en-US")
for voice in voices:
    print(voice.name)
```
You can use the `synthesize_speech_to_file` method to save an audio file:
```python
result = speech_synthesizer.synthesize_speech_to_file("Hello, how are you doing?", "output.wav")
```
## 4. Translation
To translate text using the Azure Translator Text API, you can use the TranslationServiceClient class from the azure.ai.textanalytics module. Here's an example of how to translate a text from English to French:
```python
from azure.ai.textanalytics import TranslationServiceClient
from azure.core.credentials import AzureKeyCredential

# Set up the Translation API client
key = "YourTranslationServicesApiKey"
endpoint = "YourTranslationServicesEndpoint"
credential = AzureKeyCredential(key)
translation_client = TranslationServiceClient(endpoint, credential)

# Translate text from English to French
result = translation_client.translate_text("Hello, how are you?", target_language="fr")

# Print the translated text
print(result[0].translations[0].text)
```
