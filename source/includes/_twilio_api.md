
# Twilio API

Cadence Translate provides high-accuracy transcription and voice-over translation services for Twilio, and its partners.




## Installation and Configuration

There is no installation needed. The API  accepts requests via HTTP POST to the URL 

`https://cadencetranscription.herokuapp.com/api`




## Runtime Request Parameters

The API requires the following parameters on any request:

Parameter | Description
------------------------------- | ------
<nobr> `request_sid` </nobr>  | Unique identifier for this request  *(as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish) )*
<nobr> `callback_url` </nobr>  | Callback URL for the asynchronous response  *(as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish) )*
<nobr> `audio_data` </nobr>  | Audio file binary bytestream *(as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish) )*
<nobr> `requested_service` </nobr>  | Requested Service. Should be one of the following: [<nobr>`transcription`,</nobr> <nobr>`translation`]</nobr>
<nobr> `source_languages` </nobr> | Languages in the audio file provided by the client. Should contain a language code, or a comma separated list of language codes. E.g. `en`, `cn`, or `en,de,cn`.
<nobr> `target_language` </nobr> | Desired target language, either for the transcription or the voice-over translation. Should contain **a single language code**.



### Authentication

All requests must use **HTTP basic auth** using a username and password we provide.


## Result

For any successful asynchronous request, we return a **202 Accepted** response with no body. 

### Callback response

After completion of the demanded service, we POST a request to the `callback_url`.


Parameter | Description
------------------------------- | ------
<nobr> `request_sid` </nobr>  | The unique identifier for this request
<nobr> `status` </nobr> | Status message. Either `ok`, or `error`
<nobr> `message` </nobr> | An empty string, or a message explaing the related status code.
<nobr> `transcription` </nobr> | *optional* If requested, a document containing the transcription.
<nobr> `translation` </nobr> | *optional* If requested, an audio file with voice-over translation of the original message.



### Authentication

Our request uses **HTTP basic auth** with a username and password you provide.



## Complete Sample Request 

```python

import requests

url = "https://twilioapi.herokuapp.com/api"

headers = {'X-Twilio-VendorAccountSid': 'AC05b3911315a1322d1dede66eed740000',
           'X-Twilio-Signature': '0FqS203W44/lM2UEM+51hRzwat4=',
           'X-Twilio-RequestSid': 'MR000009775bb6d43d1cabc4955723fae1',
           'X-Twilio-AddOnSid': 'XBc6dc06ce91d566fae284bc2bf36218a4',
           'X-Twilio-AddOnVersionSid': 'XC2ad3d7d6478a2ca72f224d817a241586',
           'X-Twilio-AddOnInstallSid': 'XDe2767c53b3d7be099a825252c6cf4e59',
           'X-Twilio-AddOnConfigurationSid': 'XEbee2b4cf26384f0b88ad98a25530c338',
            }

data = {'source_languages': 'en',
        'target_language': 'cn',
        'request_sid': 'MR000009775bb6d43d1cabc4955723fae1',
        'requested_service': 'transcription',
        'callback_url': 'http://httpbin.org/post',
        'channels': 2,
        'duration': 6000, # in milliseconds
        'format': 'audio/x-wav',
        'media_sid': 'WcS42767c53b3d7be099a825252c6c123e59',
        'size': 1204, # in bytes
        }

auth = ('cadence', 'friend')

audio_file = open('file.wav', 'rb')
files = {'audio_data': audio_file}

r = requests.post(url, auth=auth, files=files, data=data, headers=headers)

assert r.status_code == 202
assert not r.content

```



## Complete Sample Response


```python

import requests

callback_url = 'http://httpbin.org/post'

headers = {'user-agent': 'cadencetranscription/v/0.1'}

data = {
    'request_sid': 'MR000009775bb6d43d1cabc4955723fae1',
    'status': 'ok',
    'message': '',
}

auth = ('cadence', 'friend')

audio_file = open('file.wav', 'rb')
transcription_file = open('transcription.pdf', 'rb')

files = {'transcription': transcription_file,
         'translation': audio_file}

r = requests.post(url, auth=auth, files=files, data=data, headers=headers)
```


