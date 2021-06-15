# LibreTranslate-py
Python bindings for LibreTranslate

## Install
```
pip install libretranslatepy
```

## Example usage
```
from libretranslatepy import LibreTranslateAPI
lt = LibreTranslateAPI("https://translate.astian.org/")
print(lt.detect("Hello World"))
print(lt.languages())
print(lt.translate("LibreTranslate is awesome!", "en", "es"))
```

## Source
```
import json
import sys
from urllib import request, parse

class LibreTranslateAPI:
    """Connect to the LibreTranslate API"""

    """Example usage:
    from libretranslatepy import LibreTranslateAPI
    lt = LibreTranslateAPI("https://translate.astian.org/")
    print(lt.detect("Hello World"))
    print(lt.languages())
    print(lt.translate("LibreTranslate is awesome!", "en", "es"))
    """

    DEFAULT_URL = "https://translate.astian.org/"

    def __init__(self, url=None):
        self.url = DEFAULT_URL if url is None else url
        assert len(self.url) > 0

        # Add trailing slash
        if self.url[-1] != "/":
            self.url += "/"

    def translate(self, q, source="en", target="es"):
        """Translate string
        Args:
            q (str): The text to translate
            source (str): The source language code (ISO 639)
            target (str): The target language code (ISO 639)
        Returns: The translated text
        """

        url = self.url + "translate"

        params = {"q": q, "source": source, "target": target}

        url_params = parse.urlencode(params)

        req = request.Request(url, data=url_params.encode())

        response = request.urlopen(req)

        response_str = response.read().decode()

        return json.loads(response_str)["translatedText"]

    def languages(self):
        """Retrieve list of supported languages
        Returns: A list of available languages ex. [{"code":"en", "name":"English"}]
        """

        url = self.url + "languages"

        req = request.Request(url, method="POST")

        response = request.urlopen(req)

        response_str = response.read().decode()

        return json.loads(response_str)

    def detect(self, q):
        """Detect the language of a single text
        Args:
            q (str): Text to detect
        Returns: The detected languages ex. [{"confidence": 0.6, "language": "en"}]
        """

        url = self.url + "detect"

        params = {"q": q}

        url_params = parse.urlencode(params)

        req = request.Request(url, data=url_params.encode())

        response = request.urlopen(req)

        response_str = response.read().decode()

        return json.loads(response_str)


```

## License
Dual licensed under either the MIT License or CC0.

