# Azure-speech-service

* The extensive documentation is located [here](https://learn.microsoft.com/en-us/python/api/azure-cognitiveservices-speech/?view=azure-python).
* The supported voices and languages are listed [here](https://learn.microsoft.com/en-us/azure/cognitive-services/speech-service/language-support?tabs=tts).

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
result = speech_recognizer.recognize_once()
print(result.text)
```
This code sets up the Speech SDK configuration and audio configuration, creates a SpeechRecognizer object, and then calls the `recognize_once` method to transcribe the spoken language to text. The `use_default_microphone` parameter in the `AudioConfig` constructor specifies that the Speech SDK should use the default microphone on the device.

## 3. Text to voice
The Azure Speech Services can also be used to convert text to spoken language. This is useful for building applications that can generate speech in real-time. Here is how you can use the Speech SDK to convert text to spoken language:
```python
import azure.cognitiveservices.speech as speechsdk

# Set up the Speech SDK configuration
speech_key = SPEECH_KEY
service_region = SPEECH_REGION

audio_config = speechsdk.audio.AudioOutputConfig(use_default_speaker=True)

speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
speech_config.speech_synthesis_voice_name = 'en-US-JennyNeural'

speech_synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config, audio_config=audio_config)

speech_synthesis_result = speech_synthesizer.speak_text_async("Hey man!").get()

# Play audio
audio_player = speechsdk.AudioPlayer()
audio_player.play_audio(result.audio_data)
```
In this example, the speech_synthesis_language property is set to "en-US" to specify that the text should be synthesized in English. You can find a list of supported language codes in the [Azure Text-to-Speech documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/language-support). 
You can use change `AudioOutputConfig` to save an audio file:
```python
import azure.cognitiveservices.speech as speechsdk

# Set up the Speech SDK configuration
speech_key = SPEECH_KEY
service_region = SPEECH_REGION

audio_config = speechsdk.audio.AudioOutputConfig(filename="file.wav")

speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
speech_config.speech_synthesis_voice_name = 'en-US-JennyNeural'

speech_synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config, audio_config=audio_config)

speech_synthesis_result = speech_synthesizer.speak_text_async("Hey man!").get()
```
## 4. Translation
To translate recognize an audio from a microphone and translate into other language, `translation` will be used. As an example, translate Argentinan spanish to english:
```python
import azure.cognitiveservices.speech as speechsdk

# Set up the Speech SDK configuration
speech_key = SPEECH_KEY
service_region = SPEECH_REGION
source_lang = "es-AR"
target_lang = "en"

audio_config = speechsdk.AudioConfig(use_default_microphone=True)

speech_translation_config = speechsdk.translation.SpeechTranslationConfig(subscription=SPEECH_KEY, region=SPEECH_REGION)
speech_translation_config.speech_recognition_language = source_lang
speech_translation_config.add_target_language(target_lang)

translation_recognizer = speechsdk.translation.TranslationRecognizer(translation_config=speech_translation_config, audio_config=audio_config)

result = translation_recognizer.recognize_once_async().get()
print(result.translations[target_lang])
```
