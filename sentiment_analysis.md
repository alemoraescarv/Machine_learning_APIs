```python
from google.cloud import language_v1

def sample_analyze_sentiment():
    """
    Analyzing Sentiment in a String
    Args:
      text_content The text content to analyze
    """

    client = language_v1.LanguageServiceClient()

    text_content = 'I am so happy and joyful.'

    # Available types: PLAIN_TEXT, HTML
    type_ = language_v1.Document.Type.PLAIN_TEXT

    # Optional. If not specified, the language is automatically detected.
    # For list of supported languages:
    # https://cloud.google.com/natural-language/docs/languages
    language = "en"
    document = {"content": text_content, "type_": type_, "language": language}

    # Available values: NONE, UTF8, UTF16, UTF32
    encoding_type = language_v1.EncodingType.UTF8

    response = client.analyze_sentiment(request = {'document': document, 'encoding_type': encoding_type})
    # Get overall sentiment of the input document
    print(u"Document sentiment score: {}".format(response.document_sentiment.score))
    print(
        u"Document sentiment magnitude: {}".format(
            response.document_sentiment.magnitude
        )
    )
    # Get sentiment for all sentences in the document
    for sentence in response.sentences:
        print(u"Sentence text: {}".format(sentence.text.content))
        print(u"Sentence sentiment score: {}".format(sentence.sentiment.score))
        print(u"Sentence sentiment magnitude: {}".format(sentence.sentiment.magnitude))

    # Get the language of the text, which will be the same as
    # the language specified in the request or, if not specified,
    # the automatically-detected language.
    print(u"Language of the text: {}".format(response.language))


# [END language_sentiment_text]


def main():


    sample_analyze_sentiment()


if __name__ == "__main__":
    main()
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-6-1cc492eb203d> in <module>()
         54 
         55 if __name__ == "__main__":
    ---> 56     main()
    

    <ipython-input-6-1cc492eb203d> in main()
         50 
         51 
    ---> 52     sample_analyze_sentiment()
         53 
         54 


    <ipython-input-6-1cc492eb203d> in sample_analyze_sentiment()
         13 
         14     # Available types: PLAIN_TEXT, HTML
    ---> 15     type_ = language_v1.Document.Type.PLAIN_TEXT
         16 
         17     # Optional. If not specified, the language is automatically detected.


    AttributeError: module 'google.cloud.language_v1' has no attribute 'Document'



```python
!pip install --upgrade google-cloud-language

```

    Collecting google-cloud-language
      Downloading google_cloud_language-2.0.0-py2.py3-none-any.whl (149 kB)
    [K     |████████████████████████████████| 149 kB 8.8 MB/s eta 0:00:01
    [?25hCollecting proto-plus>=1.10.0
      Downloading proto_plus-1.15.0-py3-none-any.whl (42 kB)
    [K     |████████████████████████████████| 42 kB 1.4 MB/s  eta 0:00:01
    [?25hCollecting google-api-core[grpc]<2.0.0dev,>=1.22.2
      Downloading google_api_core-1.26.1-py2.py3-none-any.whl (92 kB)
    [K     |████████████████████████████████| 92 kB 1.1 MB/s  eta 0:00:01
    [?25hCollecting libcst>=0.2.5
      Downloading libcst-0.3.17-py3-none-any.whl (507 kB)
    [K     |████████████████████████████████| 507 kB 11.3 MB/s eta 0:00:01
    [?25hRequirement already satisfied, skipping upgrade: protobuf>=3.12.0 in /opt/conda/lib/python3.7/site-packages (from proto-plus>=1.10.0->google-cloud-language) (3.12.2)
    Requirement already satisfied, skipping upgrade: pytz in /opt/conda/lib/python3.7/site-packages (from google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (2019.3)
    Requirement already satisfied, skipping upgrade: packaging>=14.3 in /opt/conda/lib/python3.7/site-packages (from google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (20.1)
    Requirement already satisfied, skipping upgrade: googleapis-common-protos<2.0dev,>=1.6.0 in /opt/conda/lib/python3.7/site-packages (from google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (1.51.0)
    Requirement already satisfied, skipping upgrade: six>=1.13.0 in /opt/conda/lib/python3.7/site-packages (from google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (1.14.0)
    Requirement already satisfied, skipping upgrade: requests<3.0.0dev,>=2.18.0 in /opt/conda/lib/python3.7/site-packages (from google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (2.23.0)
    Requirement already satisfied, skipping upgrade: setuptools>=40.3.0 in /opt/conda/lib/python3.7/site-packages (from google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (46.1.1.post20200322)
    Collecting google-auth<2.0dev,>=1.21.1
      Downloading google_auth-1.27.1-py2.py3-none-any.whl (136 kB)
    [K     |████████████████████████████████| 136 kB 16.1 MB/s eta 0:00:01
    [?25hRequirement already satisfied, skipping upgrade: grpcio<2.0dev,>=1.29.0; extra == "grpc" in /opt/conda/lib/python3.7/site-packages (from google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (1.29.0)
    Collecting typing-extensions>=3.7.4.2
      Downloading typing_extensions-3.7.4.3-py3-none-any.whl (22 kB)
    Collecting typing-inspect>=0.4.0
      Downloading typing_inspect-0.6.0-py3-none-any.whl (8.1 kB)
    Requirement already satisfied, skipping upgrade: pyyaml>=5.2 in /opt/conda/lib/python3.7/site-packages (from libcst>=0.2.5->google-cloud-language) (5.3.1)
    Requirement already satisfied, skipping upgrade: pyparsing>=2.0.2 in /opt/conda/lib/python3.7/site-packages (from packaging>=14.3->google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (2.4.6)
    Requirement already satisfied, skipping upgrade: idna<3,>=2.5 in /opt/conda/lib/python3.7/site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (2.9)
    Requirement already satisfied, skipping upgrade: certifi>=2017.4.17 in /opt/conda/lib/python3.7/site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (2019.11.28)
    Requirement already satisfied, skipping upgrade: chardet<4,>=3.0.2 in /opt/conda/lib/python3.7/site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (3.0.4)
    Requirement already satisfied, skipping upgrade: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /opt/conda/lib/python3.7/site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (1.25.7)
    Requirement already satisfied, skipping upgrade: cachetools<5.0,>=2.0.0 in /opt/conda/lib/python3.7/site-packages (from google-auth<2.0dev,>=1.21.1->google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (3.1.1)
    Requirement already satisfied, skipping upgrade: pyasn1-modules>=0.2.1 in /opt/conda/lib/python3.7/site-packages (from google-auth<2.0dev,>=1.21.1->google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (0.2.7)
    Requirement already satisfied, skipping upgrade: rsa<5,>=3.1.4; python_version >= "3.6" in /opt/conda/lib/python3.7/site-packages (from google-auth<2.0dev,>=1.21.1->google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (4.0)
    Requirement already satisfied, skipping upgrade: mypy-extensions>=0.3.0 in /opt/conda/lib/python3.7/site-packages (from typing-inspect>=0.4.0->libcst>=0.2.5->google-cloud-language) (0.4.3)
    Requirement already satisfied, skipping upgrade: pyasn1<0.5.0,>=0.4.6 in /opt/conda/lib/python3.7/site-packages (from pyasn1-modules>=0.2.1->google-auth<2.0dev,>=1.21.1->google-api-core[grpc]<2.0.0dev,>=1.22.2->google-cloud-language) (0.4.8)
    [31mERROR: kubernetes 10.1.0 has requirement pyyaml~=3.12, but you'll have pyyaml 5.3.1 which is incompatible.[0m
    Installing collected packages: proto-plus, google-auth, google-api-core, typing-extensions, typing-inspect, libcst, google-cloud-language
      Attempting uninstall: google-auth
        Found existing installation: google-auth 1.20.0
        Uninstalling google-auth-1.20.0:
          Successfully uninstalled google-auth-1.20.0
      Attempting uninstall: google-api-core
        Found existing installation: google-api-core 1.22.0
        Uninstalling google-api-core-1.22.0:
          Successfully uninstalled google-api-core-1.22.0
      Attempting uninstall: typing-extensions
        Found existing installation: typing-extensions 3.7.4.1
        Uninstalling typing-extensions-3.7.4.1:
          Successfully uninstalled typing-extensions-3.7.4.1
      Attempting uninstall: google-cloud-language
        Found existing installation: google-cloud-language 1.3.0
        Uninstalling google-cloud-language-1.3.0:
          Successfully uninstalled google-cloud-language-1.3.0
    Successfully installed google-api-core-1.26.1 google-auth-1.27.1 google-cloud-language-2.0.0 libcst-0.3.17 proto-plus-1.15.0 typing-extensions-3.7.4.3 typing-inspect-0.6.0



```python
# Imports the Google Cloud client library
from google.cloud import language_v1


# Instantiates a client
client = language_v1.LanguageServiceClient()

# The text to analyze
text = u"Hello, world!"
document = language_v1.Document(content=text, type_=language_v1.Document.Type.PLAIN_TEXT)

# Detects the sentiment of the text
sentiment = client.analyze_sentiment(request={'document': document}).document_sentiment

print("Text: {}".format(text))
print("Sentiment: {}, {}".format(sentiment.score, sentiment.magnitude))
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-9-50ba1e7c5c96> in <module>()
          8 # The text to analyze
          9 text = u"Hello, world!"
    ---> 10 document = language_v1.Document(content=text, type_=language_v1.Document.Type.PLAIN_TEXT)
         11 
         12 # Detects the sentiment of the text


    AttributeError: module 'google.cloud.language_v1' has no attribute 'Document'

