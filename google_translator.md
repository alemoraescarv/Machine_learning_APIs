```python
    !pip install google_trans_new
```

    Collecting google_trans_new
      Downloading google_trans_new-1.1.9-py3-none-any.whl (9.2 kB)
    Installing collected packages: google-trans-new
    Successfully installed google-trans-new-1.1.9



```python
from google_trans_new import google_translator  
  
translator = google_translator()
translate_text = translator.translate('how are you', lang_src='en', lang_tgt='hi', pronounce=True)  
print(translate_text)#bhai kese ho
```

    ['क्या हाल है आ ', None, 'kya haal hai aa']

