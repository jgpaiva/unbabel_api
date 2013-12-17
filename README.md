Unbabel API
===========

This github project contains the documentation for interacting with the Unbabel API.

Authentication
================================

Unbabel supports an api key-based authentication mechanism. The key is the api key associated with an account and can be obtained in the unbabel website on your profile. This key should be passed as a request header with each request. All API calls described in this document require authentication.


Translation
============

Request Translation
-------------


```shell
curl -H "Authorization: ApiKey username:f4c76b650164c179cc1b199c98ea8ef75d7dbfa6" 
     -H "Content-Type: application/json" 
     -X POST http://www.unbabel.co/tapi/v2/translation/ 
     --data 'data'
```

Where data is a json file which accepts the following arguments:

* text (required) - the text to be translated.
* target_language (required) - the language to translate the text to.
* source_language - the language of text. If not supplemented it will be auto-detected from the text.
* 

Example:

```json
{   
    "text":"In the era of Siri",
    "target_language":"pt",
}
```

Response:

```json
{
    "status": "new",
    "target_language": "pt",
    "text": "My first api test",
    "uid": "5d10df62d3"
}
```



Language Pairs
================================

Language pairs currently available in the unbabel platform:

```shell
curl -H "Authorization: ApiKey username:f4c76b650164c179cc1b199c98ea8ef75d7dbfa6" 
     -H "Content-Type: application/json" 
     -X GET http://www.unbabel.co/tapi/v2/language_pair/ 
```

Response:

This query returns a list of all language pairs currently supported by the unbabel platform. Each language pair entry contains the iso639-1 language code and language full name for both the source and target language.

```json
{
    "objects": [
                {
                    "lang_pair": {  
                                    "source_language": {"name": "Portuguese", "shortname": "pt"}, 
                                    "target_language": {"name": "English", "shortname": "en"}
                                 }
                }, 
                ...
                {
                    "lang_pair": {  
                                    "source_language": {"name": "German", "shortname": "de"}, 
                                    "target_language": {"name": "Portuguese", "shortname": "pt"}
                                 }
                }, 
                ]
}
```


Tones
================================

Language Tones available in the unbabel platform:

```shell
curl -H "Authorization: ApiKey username:f4c76b650164c179cc1b199c98ea8ef75d7dbfa6" 
     -H "Content-Type: application/json" 
     -X GET http://www.unbabel.co/tapi/v2/tone/ 
```

Response:

This query returns a list of all language tones supported by the unbabel platform. 

```json

{
    "objects": [
                {"tone": {"description": "Informal style", "name": "Informal"}}, 
                {"tone": {"description": "Friendly style", "name": "Friendly"}}, 
                {"tone": {"description": "Business style", "name": "Business"}}, 
                {"tone": {"description": "Formal style", "name": "Formal"}}
                ]
}

```
