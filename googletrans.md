```python

!pip install googletrans==3.1.0a0
```

    Collecting googletrans==3.1.0a0
      Downloading googletrans-3.1.0a0.tar.gz (19 kB)
    Requirement already satisfied: httpx==0.13.3 in /opt/conda/lib/python3.7/site-packages (from googletrans==3.1.0a0) (0.13.3)
    Requirement already satisfied: chardet==3.* in /opt/conda/lib/python3.7/site-packages (from httpx==0.13.3->googletrans==3.1.0a0) (3.0.4)
    Requirement already satisfied: sniffio in /opt/conda/lib/python3.7/site-packages (from httpx==0.13.3->googletrans==3.1.0a0) (1.2.0)
    Requirement already satisfied: rfc3986<2,>=1.3 in /opt/conda/lib/python3.7/site-packages (from httpx==0.13.3->googletrans==3.1.0a0) (1.5.0)
    Requirement already satisfied: certifi in /opt/conda/lib/python3.7/site-packages (from httpx==0.13.3->googletrans==3.1.0a0) (2019.11.28)
    Requirement already satisfied: hstspreload in /opt/conda/lib/python3.7/site-packages (from httpx==0.13.3->googletrans==3.1.0a0) (2020.12.22)
    Requirement already satisfied: idna==2.* in /opt/conda/lib/python3.7/site-packages (from httpx==0.13.3->googletrans==3.1.0a0) (2.9)
    Requirement already satisfied: httpcore==0.9.* in /opt/conda/lib/python3.7/site-packages (from httpx==0.13.3->googletrans==3.1.0a0) (0.9.1)
    Requirement already satisfied: h11<0.10,>=0.8 in /opt/conda/lib/python3.7/site-packages (from httpcore==0.9.*->httpx==0.13.3->googletrans==3.1.0a0) (0.9.0)
    Requirement already satisfied: h2==3.* in /opt/conda/lib/python3.7/site-packages (from httpcore==0.9.*->httpx==0.13.3->googletrans==3.1.0a0) (3.2.0)
    Requirement already satisfied: hpack<4,>=3.0 in /opt/conda/lib/python3.7/site-packages (from h2==3.*->httpcore==0.9.*->httpx==0.13.3->googletrans==3.1.0a0) (3.0.0)
    Requirement already satisfied: hyperframe<6,>=5.2.0 in /opt/conda/lib/python3.7/site-packages (from h2==3.*->httpcore==0.9.*->httpx==0.13.3->googletrans==3.1.0a0) (5.2.0)
    Building wheels for collected packages: googletrans
      Building wheel for googletrans (setup.py) ... [?25ldone
    [?25h  Created wheel for googletrans: filename=googletrans-3.1.0a0-py3-none-any.whl size=16367 sha256=dfca88984384bab8f4bcf696d01e0f2ad13324eb061bc586459730f46e5526ff
      Stored in directory: /home/jupyter/.cache/pip/wheels/0c/be/fe/93a6a40ffe386e16089e44dad9018ebab9dc4cb9eb7eab65ae
    Successfully built googletrans
    Installing collected packages: googletrans
      Attempting uninstall: googletrans
        Found existing installation: googletrans 3.0.0
        Uninstalling googletrans-3.0.0:
          Successfully uninstalled googletrans-3.0.0
    Successfully installed googletrans-3.1.0a0



```python
from googletrans import Translator

translator = Translator()

translator.translate('hi',src='en' ,dest='hi')
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-5-9b4ed8b949b0> in <module>()
          3 translator = Translator()
          4 
    ----> 5 translator.translate('hi',src='en' ,dest='hi')
    

    /opt/conda/lib/python3.7/site-packages/googletrans/client.py in translate(self, text, dest, src)
        170             >>> translator.translate('안녕하세요.', dest='ja')
        171             <Translated src=ko dest=ja text=こんにちは。 pronunciation=Kon'nichiwa.>
    --> 172             >>> translator.translate('veritas lux mea', src='la')
        173             <Translated src=la dest=en text=The truth is my light pronunciation=The truth is my light>
        174 


    /opt/conda/lib/python3.7/site-packages/googletrans/client.py in _translate(self, text, dest, src)
         73             #default way of working: use the defined values from user app
         74             self.service_urls = service_urls
    ---> 75             self.client_type = 'webapp'
         76             self.token_acquirer = TokenAcquirer(
         77                 client=self.client, host=self.service_urls[0])


    /opt/conda/lib/python3.7/site-packages/googletrans/gtoken.py in do(self, text)
        184                 e.append(l & 63 | 128)
        185             g += 1
    --> 186         a = b
        187         for i, value in enumerate(e):
        188             a += value


    /opt/conda/lib/python3.7/site-packages/googletrans/gtoken.py in _update(self)
         63             code = self.RE_TKK.search(r.text).group(1).replace('var ', '')
         64             # unescape special ascii characters such like a \x3d(=)
    ---> 65             code = code.encode().decode('unicode-escape')
         66         except AttributeError:
         67             raise Exception('Could not find TKK token for this request.\nSee https://github.com/ssut/py-googletrans/issues/234 for more details.')


    AttributeError: 'NoneType' object has no attribute 'group'



```python
from googletrans import Translator
translator = Translator()
translator.translate('안녕하세요.', dest='en')
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-11-56108688f91a> in <module>()
          1 from googletrans import Translator
          2 translator = Translator()
    ----> 3 translator.translate('안녕하세요.', dest='en')
    

    /opt/conda/lib/python3.7/site-packages/googletrans/client.py in translate(self, text, dest, src, **kwargs)
        180 
        181         origin = text
    --> 182         data = self._translate(text, dest, src, kwargs)
        183 
        184         # this code will be updated when the format is changed.


    /opt/conda/lib/python3.7/site-packages/googletrans/client.py in _translate(self, text, dest, src, override)
         76 
         77     def _translate(self, text, dest, src, override):
    ---> 78         token = self.token_acquirer.do(text)
         79         params = utils.build_params(query=text, src=src, dest=dest,
         80                                     token=token, override=override)


    /opt/conda/lib/python3.7/site-packages/googletrans/gtoken.py in do(self, text)
        192 
        193     def do(self, text):
    --> 194         self._update()
        195         tk = self.acquire(text)
        196         return tk


    /opt/conda/lib/python3.7/site-packages/googletrans/gtoken.py in _update(self)
         60 
         61         # this will be the same as python code after stripping out a reserved word 'var'
    ---> 62         code = self.RE_TKK.search(r.text).group(1).replace('var ', '')
         63         # unescape special ascii characters such like a \x3d(=)
         64         code = code.encode().decode('unicode-escape')


    AttributeError: 'NoneType' object has no attribute 'group'



```python

```
