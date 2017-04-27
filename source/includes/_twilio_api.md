
# Twilio API


The purpose of this API is to allow Cadence Translate to work with Twilio, to provide transcription and dubbing services. Using a combination of manual and or machine supported translation and transcription, we can produce a high-quality result.

## Inital Request

All requests made to this API should be made via HTTP POST to the URL 

`https://twillioapi.herokuapp.com/api`

### Authentication

All requests must use **HTTP basic auth** using a username and password, which we provide on request.

### Headers 

All requests are expected to pass the following HTTP Headers:


HTTP Header Field | Description
------------------------------- | ------
<nobr>`X-Twilio-VendorAccountSid`   </nobr> | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr>`X-Twilio-Signature`  </nobr> | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr>`X-Twilio-RequestSid`   </nobr> | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr>`X-Twilio-AddOnSid`   </nobr> | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr>`X-Twilio-AddOnVersionSid`  </nobr> | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr>`X-Twilio-AddOnInstallSid`  </nobr> | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr>`X-Twilio-AddOnConfigurationSid`  </nobr> | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*


### HTTP Form data

HTTP POST data should provide the following fields.


www-form-encoded field | Description
------------------------------- | ------
<nobr> `audio_data` </nobr> | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr> `callback_url` </nobr>   | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr> `channels` </nobr>  | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr> `duration` </nobr>   | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr> `format` </nobr>   | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr> `media_sid` </nobr>  | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr> `size` </nobr>  | *as in [Twilio's documentation](https://www.twilio.com/docs/api/add-ons/publish)*
<nobr> `requested_service` </nobr>  | Should be one of the following: [<nobr>`transcription`,</nobr> <nobr>`translation`,</nobr> <nobr>`transcription_translation`]</nobr>
<nobr> `source_language` </nobr> | Describes the spoken languages in the audio file provided by the client. Should contain a language code, or a comma separated list of language codes. E.g. `en`, `cn`, or `en,de,cn`.
<nobr> `target_language` </nobr> | *(optional)* Describes the desired output languages, either for the transcription or the dubbed file. Should contain **a single language code**. A comma separated list of language codes is not accepted. If not provided, the source languages will be used.


### Immediate response

The immediate response will be an empty HttpResponse with a 202 (HTTP_ACCEPTED) status code.


> A valid request can be made using this code:

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

data = {'source_language': 'en',
        'target_language': 'cn',
        'requested_service': 'transcription',
        'callback_url': 'http://httpbin.org/post',
        'channels': 2,
        'duration': 6000, # in milliseconds
        'format': 'audio/x-wav',
        'media_sid': 'WcS42767c53b3d7be099a825252c6c123e59',
        'size': 1204, # in bytes
        }

# Replace authentication
auth = ('cadence', 'friend')

# Replace Audio File
audio_file = open('file.wav', 'rb')
files = {'audio_data': audio_file}

r = requests.post(url, auth=auth, files=files, data=data, headers=headers)

assert r.status_code == 202
assert not r.content

```


## Callback Response

After creation and manual checks the transciptions and translations, we submit a HTTP POST request to the `callback-url` that was provided earlier.


### Authentication

Authentication will be provided by HTTP basic auth using a username and password provided by Twilio.

### Header Data


HTTP Header Field | Description
------------------------------- | ------
<nobr>`user-agent`   </nobr> | Identifier of our app, e.g. `cadencetranslate/v/0.1`




### Form Data


www-form-encoded field | Description
------------------------------- | ------
<nobr> `X-Twilio-RequestSid` </nobr>  | The related RequestSid
<nobr> `status` </nobr> | A status message. Either `ok`, or `error`
<nobr> `message` </nobr> | An empty string, or a message explaing the related status code.
<nobr> `transcription` </nobr> | *optional* If requested, a document containing the transcription.
<nobr> `translation` </nobr> | *optional* If requested, an audio file where the original message is dubbed.



> A similar request can created using this code:

```python

import requests

callback_url = [...]

headers = {'user-agent': 'cadencetranslate/v/0.1'}

data = {
    'X-Twilio-RequestSid': 'MR000009775bb6d43d1cabc4955723fae1',
    'status': 'ok',
    'message': '',
}

# Replace authentication
auth = ('cadence', 'friend')

# Replace Audio File
audio_file = open('file.wav', 'rb')
transcription_file = open('transcription.pdf', 'rb')

files = {'transcription': transcription_file,
         'translation': audio_file}

r = requests.post(url, auth=auth, files=files, data=data, headers=headers)
```

