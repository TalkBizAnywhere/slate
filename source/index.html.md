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

# Call Booking API

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

## Book a new call

```shell
curl -X POST -H "Content-Type: application/json" -H "Authorization: meowmeowmeow" '{"api_token": "meowmeowmeow"
, "start_time": "2017-02-14T13:15:03Z"
, "timezone": "Asia/Bangkok"
, "languages" : [1,20]
, "notes" : "This is a great job"}' "https://cadence2.bubbleapps.io/api/1.1/wf/create_job"
```

> The above command returns JSON structured like this:

```json
{
    "status": "success",
    "response": {
        "job_id": "1486939273049x736883949009633300",
        "job_start_time": "Friday, February 17, 2017 11:15 am",
        "job_timezone": "Europe/Lisbon",
        "job_duration": "10 to 60 minutes",
        "job_pin": 864226,
        "job_languages": "English, Malaysian",
        "job_active": true,
        "user_id": "1487026541060x869803193258121600"
    }
}
```

This endpoint creates a new call with given start time, timezone and languages on Cadence's platform.

### HTTP Request

`POST https://cadence2.bubbleapps.io/api/1.1/wf/create_job`

### URL Parameters

Parameter | Description
--------- | -----------
api_token | Pre-assigned API token for authentication.
start_time | The date and time at which the call will happen, expected in format YYYY-MM-DDTHH:mm:ssZ. Note the Z value does not have to be the timezone in which the local time is as long as the timezone field is specified.
timezone | A string containing the "tz" ID of the local timezone in which the call will happen, such as "America/Los_Angeles" or "Asia/Shanghai", as defined in IANA Time Zone Database and a list of available values can be found on [Wikipedia](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).
languages | A list of language IDs will be used in the call, e.g. [1,2,6] stands for English, Mandarin and French. Accepted languages and their ids can be found [here](https://cadence2.bubbleapps.io/api/1.1/obj/language) (JSON)
notes    | Any additional information we need to know in order to serve you better.

### Response

Property | Description
--------- | -----------
response | JSON object of the response, with fields listed below.
status | Indicating request success or failure.

#### Fields of response object

Property | Description
--------- | -----------
job_id | The unique id of the created job.
job_start_time | The date and time at which the call will happen.
job_timezone | The local timezone in which the call will happen.
job_duration | How long would the call take.
job_pin | A PIN for dial-in numbers.
job_languages | Languages will be used in the call.
job_active | Boolean value indicates whether the call is cancelled.