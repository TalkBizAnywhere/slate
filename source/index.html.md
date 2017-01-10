---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='#authentication'>Request a new API Key</a>
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

API keys are used to authenticate users of our API. You can [contact us](mailto:matt@talkbusinessanywhere.com) to request a new API key.

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
, "industry": 142 }' "https://smart-match-staging.herokuapp.com/get_price"
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
quality | Represents how important industry knowledge is to the match. Accepted values are (1, 2, 3) and must be passed as an integer. 1 indicates "low priority", 2 indicates "medium priority" and 3 indicates "high priority".
price | Represents an approximation of the client's budget. Accepted values are (1, 2, 3) and must be passed as an integer. 1 indicates "value matches" are desired by the client, 2 indicates "standard matches" are desired by the client, 3 indicates the client wants to see "premium matches".
speed | Accepted values are (-1, 0, 1) and must be passed as an integer. This is a proxy for urgency (-1 indicates "not urgent", 0 is "normal", and 1 is "very urgent"). This is being deprecated in favor of a proper date/time field which will be documented shortly.
location | The ID of the location where the job will take place, a list of accepted locations and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/locations) (JSON). For on the phone interpretations and translations, please use id **1374**.
service_type | *"interpretation"* (e.g., phone calls or live meetings) or *"translation"* (e.g., documents)
target_language | The ID of the language the target audience speak, a list of accepted languages and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/languages) (JSON)
source_language | The ID of the language the speaker uses, a list of accepted languages and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/languages) (JSON)
industry | The ID of the related industry this job belongs to. A list of accepted industries and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/industries) (JSON)

### Response

Property | Description
--------- | -----------
avatars | An array of avatar urls of potential candidates.
price | Estimated price in USD (per hour if *interpretation*, per 100 words if *translation*)
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
, "industry": 142 }' "https://smart-match-staging.herokuapp.com/submit_job"
```

> The above command returns JSON structured like this:

```json
{
  "status": "OK"
}
```
<aside class="notice">
This endpoint verifies whether the parameters are valid for creating a new job. The current naming of this endpoint will change: *submit_job* will become *validate_job*.
</aside>

### HTTP Request

`POST https://smart-match-staging.herokuapp.com/submit_job`

### Query Parameters

Parameter | Description
--------- | -----------
quality | Represents how important industry knowledge is to the match. Accepted values are (1, 2, 3) and must be passed as an integer. 1 indicates "low priority", 2 indicates "medium priority" and 3 indicates "high priority".
price | Represents an approximation of the client's budget. Accepted values are (1, 2, 3) and must be passed as an integer. 1 indicates "value matches" are desired by the client, 2 indicates "standard matches" are desired by the client, 3 indicates the client wants to see "premium matches".
speed | Accepted values are (-1, 0, 1) and must be passed as an integer. This is a proxy for urgency (-1 indicates "not urgent", 0 is "normal", and 1 is "very urgent"). This is being deprecated in favor of a proper date/time field which will be documented shortly.
location | The ID of the location where the job will take place, a list of accepted locations and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/locations) (JSON). For on the phone interpretations and translations please use id **1374**.
jobid | The ID of the job for internal use, any number accepted here.
service_type | *"interpretation"* (e.g., phone calls or live meetings) or *"translation"* (e.g., documents)
target_language | The ID of the language the target audience speak, a list of accepted languages and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/languages) (JSON)
source_language | The ID of the language the speaker uses, a list of accepted languages and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/languages) (JSON)
industry | The ID of the related industry the job to be validated belongs to. A list of accepted industries and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/industries) (JSON)

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
, "industry": 142 }' "https://smart-match-staging.herokuapp.com/create_job"
```

> The above command returns JSON structured like this:

```json
{
  "job": {
    "id": 2,
    "industry": "Agriculture",
    "language_pairs": {"language_from": "English", "language_to": "Mandarin"},
    "location": "Beijing",
    "price": 75,
    "start_date": "2017-02-02",
    "title": "February 02 2017 English / Mandarin Interpreting (in-person) in Beijing for Agriculture"
  },
  "status": "OK"
}
```

This endpoint creates a new job with given location, target and source languages and industry on Cadence's platform.

<aside class="alert">
This endpoint is not yet available.
</aside>

### HTTP Request

`POST https://smart-match-staging.herokuapp.com/create_job`

### URL Parameters

Parameter | Description
--------- | -----------
quality | Represents how important industry knowledge is to the match. Accepted values are (1, 2, 3) and must be passed as an integer. 1 indicates "low priority", 2 indicates "medium priority" and 3 indicates "high priority".
price | Represents an approximation of the client's budget. Accepted values are (1, 2, 3) and must be passed as an integer. 1 indicates "value matches" are desired by the client, 2 indicates "standard matches" are desired by the client, 3 indicates the client wants to see "premium matches".
speed | Accepted values are (-1, 0, 1) and must be passed as an integer. This is a proxy for urgency (-1 indicates "not urgent", 0 is "normal", and 1 is "very urgent"). This is being deprecated in favor of a proper date/time field which will be documented shortly.
location | The ID of the location where the job will take place, a list of accepted locations and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/locations) (JSON). For on the phone interpretations and translations please use id **1374**.
date | The date on which an interpretation job will happen or a translation job will be due. If no value was passed in, the date will be considered **"TBD"**
service_type | *"interpretation"* (e.g., phone calls or live meetings) or *"translation"* (e.g., documents)
target_language | The ID of the language the target audience speak, a list of accepted languages and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/languages) (JSON)
source_language | The ID of the language the speaker uses, a list of accepted languages and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/languages) (JSON)
industry | The ID of the related industry the job to be created belongs to. A list of accepted industries and their ids can be found [here](https://app.talkbusinessanywhere.com/api/v1/industries) (JSON)

### Response

Property | Description
--------- | -----------
job | JSON object of the created job, with auto-generated **title** property contains date, language and location information.
status | Indicating request success or failure.
