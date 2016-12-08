---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='#'>Request a new API Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Cadence API! You can use our API to create new interpretation and translation jobs on [Cadence's platform](https://www.talkbusinessanywhere.com).

This example API documentation page was created with [Slate](https://github.com/tripit/slate).

# Basics

All methods must be called using HTTPS. Arguments can be passed as POST, with query params placed in the body of the JSON payload. The response contains a JSON object, which contains a top-level property status, indicating success or failure.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

API keys are used to authenticate users of our API. You can [contact us](http://support.talkbusinessanywhere.com/) to request a new API key.

The API key is expected to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Jobs API

## Get a quote for a job

```shell
curl -X POST -H "Content-Type: application/json" -H "Authorization: meowmeowmeow" -d '{"quality": 2
, "price": 1
, "speed": 2
, "location": 1
, "service_type": "interpretation"
, "target_language": 1
, "source_language": 18
, "industry": "clothing" }' "https://smart-match-staging.herokuapp.com/get_price"
```

> The above command returns JSON structured like this:

```json
{
  "avatars": {
    "37": "https://seekpanda-prod.imgix.net/415/415.jpg?1454485805716",
    "542": "0",
    "556": "https://seekpanda-prod.imgix.net/512/512.jpg?1456992837691",
    "626": "",
    "1115": "https://seekpanda-prod.imgix.net/2195/2195.jpg?1477496937121"
  },
  "price": 165,
  "status": "OK"
}
```

This endpoint returns a price generated from the parameters represent a given job.

### HTTP Request

`POST https://smart-match-staging.herokuapp.com/get_price`

### Query Parameters

Parameter | Description
--------- | -----------
quality | Accepted value range (1-3, as integer). Documentation forthcoming
price | Accepted value range (1-3, as integer). Documentation forthcoming
speed | Accepted value range (-1, 0, 1 as integer). Documentation forthcoming
location | The ID of the location where the job will take place, a list of accepted locations and their ids can be found [here](https://www.talkbusinessanywhere.com/api/v1/locations). For on the phone interpretations and translations please use id **1374**.
service_type | *"interpretation"* or *"translation"*
target_language | The ID of the language the target audience speak, a list of accepted languages and their ids can be found [here](https://www.talkbusinessanywhere.com/api/v1/languages)
source_language | The ID of the language the speaker uses, a list of accepted languages and their ids can be found [here](https://www.talkbusinessanywhere.com/api/v1/languages)
industry | The related industry of the job, in string and lower case. A list of accepted industries can be found [here](/javascripts/industries.json)

### Response

Property | Description
--------- | -----------
avatars | An array of avatar urls of potential candidates.
price | Estimated price in USD.
status | Indicating request success or failure.

## Validate job submission

```shell
curl -X POST -H "Content-Type: application/json" -H "Authorization: meowmeowmeow" -d '{"quality": 2
, "price": 1
, "speed": 2
, "jobid" : 100
, "location": 1
, "service_type": "interpretation"
, "target_language": 1
, "source_language": 10
, "industry": "clothing" }' "https://smart-match-staging.herokuapp.com/submit_job"
```

> The above command returns JSON structured like this:

```json
{
  "status": "OK"
}
```

This endpoint verifies whether the parameters are valid for creating a new job.

### HTTP Request

`POST https://smart-match-staging.herokuapp.com/submit_job`

### Query Parameters

Parameter | Description
--------- | -----------
quality | Accepted value range (1-3, as integer). Documentation forthcoming
price | Accepted value range (1-3, as integer). Documentation forthcoming
speed | Accepted value range (-1, 0, 1 as integer). Documentation forthcoming
location | The ID of the location where the job will take place, a list of accepted locations and their ids can be found [here](https://www.talkbusinessanywhere.com/api/v1/locations). For on the phone interpretations and translations please use id **1374**.
jobid | The ID of the job for internal use, any number accepted here.
service_type | *"interpretation"* or *"translation"*
target_language | The ID of the language the target audience speak, a list of accepted languages and their ids can be found [here](https://www.talkbusinessanywhere.com/api/v1/languages)
source_language | The ID of the language the speaker uses, a list of accepted languages and their ids can be found [here](https://www.talkbusinessanywhere.com/api/v1/languages)
industry | The related industry of the job, in string and lower case. A list of accepted industries can be found [here](/javascripts/industries.json)

<aside class="success">
Remember — a successful request is an authenticated one!
</aside>

## Create a new job

```shell
curl -X POST -H "Content-Type: application/json" -H "Authorization: meowmeowmeow" -d '{"quality": 2
, "price": 1
, "speed": 2
, "date" : "2016-12-21"
, "location": 1
, "service_type": "interpretation"
, "target_language": 1
, "source_language": 10
, "industry": "clothing" }' "https://smart-match-staging.herokuapp.com/create_job"
```

> The above command returns JSON structured like this:

```json
{
  "job": {
    "id": 2,
    "industry": "agriculture",
    "language_pairs": {"language_to": "Mandarin", "language_from": "English"},
    "location": "Shanghai",
    "price": 120,
    "start_date": "2016-09-27T12:00:00Z",
    "title": "September 27 2016 English / Mandarin Interpreting (in-person) in Shanghai for Agriculture"
  },
  "status": "OK"
}
```

This endpoint creates a new job with given location, target and source languages and industry on Cadence's platform.

### HTTP Request

`POST https://smart-match-staging.herokuapp.com/create_job`

### URL Parameters

Parameter | Description
--------- | -----------
quality | Accepted value range (1-3, as integer). Documentation forthcoming
price | Accepted value range (1-3, as integer). Documentation forthcoming
speed | Accepted value range (-1, 0, 1 as integer). Documentation forthcoming
location | The ID of the location where the job will take place, a list of accepted locations and their ids can be found [here](https://www.talkbusinessanywhere.com/api/v1/locations). For on the phone interpretations and translations please use id **1374**.
date | The date on which an interpretation job will happen or a translation job will be due. If no value was passed in, the date will be considered **"TBD"**
service_type | *"interpretation"* or *"translation"*
target_language | The ID of the language the target audience speak, a list of accepted languages and their ids can be found [here](https://www.talkbusinessanywhere.com/api/v1/languages)
source_language | The ID of the language the speaker uses, a list of accepted languages and their ids can be found [here](https://www.talkbusinessanywhere.com/api/v1/languages)
industry | The related industry of the job, in string and lower case. A list of accepted industries can be found [here](/javascripts/industries.json)

### Response

Property | Description
--------- | -----------
job | JSON object of the created job, with auto-generated **title** property contains date, language and location information.
status | Indicating request success or failure.
