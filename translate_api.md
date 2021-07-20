```python
!pip install google-cloud-translate==2.0.1 --quiet

```


```python

```


```python
!pip install --upgrade google-cloud-translate --quiet
```


```python


def translate_text(target, text, format):
    """Translates text into the target language.

    Target must be an ISO 639-1 language code.
    See https://g.co/cloud/translate/v2/translate-reference#supported_languages
    """
    import six
    from google.cloud import translate_v2 as translate

    translate_client = translate.Client()

    if isinstance(text, six.binary_type):
        text = text.decode("utf-8")

    # Text can also be a sequence of strings, in which case this method
    # will return a sequence of results for each text.
    result = translate_client.translate(text, target_language=target,format_=format)

    print(u"Text: {}".format(result["input"]))
    print(u"Translation: {}".format(result["translatedText"]))
    print(u"Detected source language: {}".format(result["detectedSourceLanguage"]))

text='''Ja, det hvor vi lige har været, der virker det hvis jeg åbner den sådan, men det virker ikke når jeg åbner min mail 'app' - den der ligger forneden (som er den jeg bruger normalt)'''
translate_text('en',text,'text')
```

    Text: Ja, det hvor vi lige har været, der virker det hvis jeg åbner den sådan, men det virker ikke når jeg åbner min mail 'app' - den der ligger forneden (som er den jeg bruger normalt)
    Translation: Yeah Al that sounds pretty crap to me, Looks like BT aint for me either, Looks like BT aint for me either, Looks like BT aint for me either.
    Detected source language: da



```python
def translate_text_with_model(target, text, format, model="nmt"):
    """Translates text into the target language.

    Make sure your project is allowlisted.

    Target must be an ISO 639-1 language code.
    See https://g.co/cloud/translate/v2/translate-reference#supported_languages
    """
    import six
    from google.cloud import translate_v2 as translate

    translate_client = translate.Client()

    if isinstance(text, six.binary_type):
        text = text.decode("utf-8")

    # Text can also be a sequence of strings, in which case this method
    # will return a sequence of results for each text.
    result = translate_client.translate(text, target_language=target, model=model,format_=format)

    print(u"Text: {}".format(result["input"]))
    print(u"Translation: {}".format(result["translatedText"]))
    print(u"Detected source language: {}".format(result["detectedSourceLanguage"]))

    

text='What is your IT related request ? : En la pantalla de MHS L53 aparecen pedidos internos hechos por el usuario "DUMMY" y no es un usuario de la tienda ni sabemos por qué aparece. Ya se reportó en su momento'

translate_text('en',text,'text' )    
```

    Text: What is your IT related request ? : En la pantalla de MHS L53 aparecen pedidos internos hechos por el usuario "DUMMY" y no es un usuario de la tienda ni sabemos por qué aparece. Ya se reportó en su momento
    Translation: What is your IT related request? : The MHS L53 screen shows internal orders made by the user "DUMMY" and it is not a user of the store and we do not know why it appears. It was already reported at the time
    Detected source language: es

