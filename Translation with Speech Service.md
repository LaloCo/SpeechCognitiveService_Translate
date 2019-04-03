
# Using Speech Services for Translation

Recently Microsoft discontinued its Azure Speech Translate Cognitive Service, or rhater, migrated it over to another service: the _Speech Service_.

This means that working with speech translation is now more similar to working with speech to text or text to speech. This also means that you now work through [an SDK](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/) that is available almost everywhere, making things easier.

So in this notebook I demonstrate how to do this with Python.

## Installing and importing the SDK


```python
!pip install azure-cognitiveservices-speech
```

    Requirement already satisfied: azure-cognitiveservices-speech in /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages (1.3.1)



```python
import azure.cognitiveservices.speech.translation as tl
```

## Configuring Your Service

Here you would like to change my speech_key for your own, since I may stop my service soon, and even if I don't, it is the free version, which means it is a bit limited when it comes to how many hours of audio this service can translate every month.


```python
speech_key, service_region = "7b6d5678bfef4f9ba3bf72b528a6be00", "southcentralus"

# We will be translating American English to Spanish and German (ESpañol and DEutsche)
speech_config = tl.SpeechTranslationConfig(subscription=speech_key, region=service_region, target_languages=['es', 'de'])

speech_config.speech_recognition_language = 'en-US'
```

## Creating and Using the Recognizer


```python
recognizer = tl.TranslationRecognizer(translation_config=speech_config)
```


```python
print("Say something in English to be translated into Spanish and German:")

result = recognizer.recognize_once()
```

    Say something in English to be translated into Spanish and German:



```python
result
```




    <azure.cognitiveservices.speech.translation.TranslationRecognitionResult at 0x10cb59518>



## Displaying the result

Yei! We got a result, not it is time to read what is in that ```TranslationRecognitionResult``` element


```python
import azure.cognitiveservices.speech as sp
import json
```


```python
if result.reason == sp.ResultReason.TranslatedSpeech:
    print('The translations are ready:')
    translations = json.loads(result.json)
    for translation in translations['Translation']['Translations']:
        print(translation['Language'], '-', translation['Text'])
else:
    print("We couldn't recognize this audio, please try again!")
```

    The translations are ready:
    es - ¿Debo compartir esto desde GitHub desde un bloc de notas de Azure o desde un bloc de notas de Google collab Pyhton?
    de - Soll ich das von github aus einem Azure-Notebook oder von einem Google-Kollab-Pyhton-Notebook teilen?



```python

```
