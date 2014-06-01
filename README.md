Unbabel API
===========

Unbabel: Translation as a Service

Unbabel provides human quality translation at a fraction of the cost and with fast translation times. This is the documention for the Unbabel API. 


Authentication
================================

Unbabel supports an api key-based authentication mechanism. 

Signup for Unbabel to get an api token at https://www.unbabel.com. The key is the api key associated with an account and can be obtained in the unbabel website on your profile. This key should be passed as a request header with each request. 

All API calls described in this document require authentication. 

To order a paid translation you first need to add some money to your account on Unbabel's website. If you just want to test the api, please refer to the sandbox section at the bottom of this page.

Translation
============

Request Translation
-------------


```shell
curl -H "Authorization: ApiKey username:api_token" 
     -H "Content-Type: application/json" 
     -X POST http://www.unbabel.co/tapi/v2/translation/ 
     --data 'data'
```

Where data is a json dictionary with the following attributes:

* text (required) - the text to be translated.
* target_language (required) - the language to translate the text to.
* source_language - the language of text. If not supplemented it will be auto-detected from the text.
* target_text - initial version of the text to be post-edit.
* uid - A unique identifier for the job. If one is not supplied the system will provide one. 
* callback_url - Once the job is done the result will be posted to this endpoint.
     * For instance: http://news.unbabel.co/unbabel_endpoint/    
* formality (optional) - The tone that should be used in the translation.
* instructions (optional) - Client instructions for the translator.
* topics (optional) - List of the topics of text. 
     * For instance ["politics"]   
* text_format - The format of the text to be translated [onf of text,html]. 

Example:

```json
{   
    "text":"In the era of Siri",
    "target_language":"pt",
    "callback_url": "http://news.unbabel.co/unbabel_endpoint/"
}
```

Response:

```json
{
    "status": "new",
    "text":"In the era of Siri",
    "target_language":"pt",
    "source_language": "en",
    "uid": "5d10df62d3",
    "price":5
}
```

* uid - The unique translation identifier. This will be used for the user to joaccessd the job.
* status - The current status of the job, can be one of: 
  * New - The translation has not been started yet. 
  * Translating -  The translation is being proceed.
  * Completed - The translation has been terminated. The client can access the translation.
  * Fail - Something went wrong with your order.
  * Canceled - The translation was canceled.
  * Accepted - The translation was accepted by the client. A job is accepted if the client does fill a complaint in 48 hours.
  * Rejected - The translation was rejected by the client.
* source_language - The detected source language
* price - The total price of the requested translation in euro cents.


Bulk Request Translations
-------------


```shell
curl -H "Authorization: ApiKey username:api_token" 
     -H "Content-Type: application/json" 
     -X patch http://www.unbabel.co/tapi/v2/translation/ 
     --data 'data'
```

Where data is a json dictionary of the following form:

```json
{ "objects": [
     {   
    "text":"Translation 1",
    "target_language":"pt",
     },
     ....
     {   
    "text":"Translation n",
    "target_language":"pt",
     }
     ]
}
```

Response:

```json
{ "objects": [
     {
     "client": "gracaninja", 
     "price": 2.0, 
     "status": "new", 
     "target_language": "pt", 
     "text": "translation 1", 
     "uid": "ee2578c2fd"
     },
     ....
     {
     "client": "gracaninja", 
     "price": 2.0, 
     "status": "new", 
     "target_language": "pt", 
     "text": "translation n", 
     "uid": "49e49bdc2d"}
     ]
}
```


Query a Translation
-------------

Returns the current state of a translation.

```shell
curl -H "Authorization: ApiKey username:api_token" 
     -H "Content-Type: application/json" 
     -X GET https://www.unbabel.co/tapi/v2/translation/uid/
```

Response:

```json
{
     "price": 4,
     "status": "accepted",
     "text":"In the era of Siri",
     "target_language":"pt",
     "source_language": "en",
     "translatedText": "teste super legal ordem.",
     "uid": "29de9551d9"
     
}
```


Query all Translations
-------------

Returns a list of translations done by this user. 
An optional query parameter can be passed to select translations with a given status: 
     * new - The translation has been created and is being pre-processed.
     * ready - The translation is ready to be processed in the unbabel platform.
     * processing - The translation is being executed.
     * delivered - The translation has already been returned to the client (either using the endpoint or query for a translation).



```shell
curl -H "Authorization: ApiKey username:api_token" 
     -H "Content-Type: application/json" 
     -X GET https://www.unbabel.co/tapi/v2/translation/?status=ready
```

Response:

```json

{
     "meta": {
               "limit": 20,
               "next": "/tapi/v2/translation/?limit=20&offset=20",
               "offset": 0,
               "previous": null,
               "total_count": 45
          },
     "objects": [
          {
               "price": 5,
               "status": "ready",
               "text": "This is a test task ",
               "uid": "a281dab6e1"
          },
          {
               "price": 4,
               "status": "ready",
               "text":"In the era of Siri",
               "target_language":"pt",
               "source_language": "en",
               "uid": "29de9551d9"
          }

     ...
     ]
}
```

Report a Translation
================================

(Not yet implemented. If you need to report a job please email api@unbabel.com and we will take care of that manually.)

Currently we handle reports on a one by one basis so a staff member will
review every report and take the appropriate action

How to report (reject) a translation:

```shell
curl -H "Authorization: ApiKey username:api_token" 
     -H "Content-Type: application/json" 
     -X POST https://www.unbabel.co/tapi/v2/report/ 
     --data 'data'
```

Where data is a json dictionary with the following attributes:

* uid (required) - the job unique identifier.
* report_type - The reason for rejecting
     * fluency
     * grammar
     * meaning
     * other
* comments - Comments on the reason.

Example:

```json
{   
    "uid": "29de9551d9"
    "report_type": "fluency",
    "comments": "wrong tone"
}
```


Language Pairs
================================

Language pairs currently available in the unbabel platform:

```shell
curl -H "Authorization: ApiKey username:api_token" 
     -H "Content-Type: application/json" 
     -X GET https://www.unbabel.co/tapi/v2/language_pair/ 
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
curl -H "Authorization: ApiKey username:api_token" 
     -H "Content-Type: application/json" 
     -X GET https://www.unbabel.co/tapi/v2/tone/ 
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



Topics
================================

Language Topics available in the unbabel platform:

```shell
curl -H "Authorization: ApiKey username:api_token" 
     -H "Content-Type: application/json" 
     -X GET https://www.unbabel.co/tapi/v2/topic/ 
```

Response:

This query returns a list of all topics supported by the unbabel platform. 

```json

{
    "objects": [
                    {"topic": {"name": "politics"}}, 
                    {"topic": {"name": "gossip"}}
                ]
}

```

Sandbox
================================

If you want to use the api in sandbox mode please email api@unbabel.com with that request. 
We will create a user, top up your account with credits and send you and api token. 
All the api calls should be made using the following url: http://sandbox.unbabel.com .
Note that tasks performed on sandbox mode will not pass by the crowd of translators. To mimic the behaviour of the system, the result of Machine Translation will be made available (either by using the endpoint, or by fetching a translation) 1 minute after the task submission. 

SDKs
================================

* Python - https://github.com/Unbabel/unbabel-py

