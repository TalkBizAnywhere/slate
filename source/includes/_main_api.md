
# Basics

All methods must be called using HTTPS. Arguments can be passed as POST, with query params placed in the body of the JSON payload. The response contains a JSON object, which contains a top-level property status, indicating success or failure.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Bearer meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

API keys are used to authenticate users of our API. You can [contact us](mailto:matt@talkbusinessanywhere.com) to request a new API key.

The API key is expected to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Call Booking API

## Book a new call

```shell
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer meowmeowmeow" '{"api_token": "meowmeowmeow"
, "start_time": "2017-02-14T13:15:03Z"
, "timezone": "Asia/Bangkok"
, "languages" : [1,20]
, "notes" : "This is a great job"}' "https://cadence2.bubbleapps.io/version-test/api/1.1/wf/create_job"
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
        "job_languages": "English, Malaysian",
        "job_active": true,
        "user_id": "1487026541060x869803193258121600"
    }
}
```

This endpoint creates a new call with given start time, timezone and languages on Cadence's platform.

### HTTP Request

`POST https://cadence2.bubbleapps.io/version-test/api/1.1/wf/create_job`

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
job_languages | Languages will be used in the call.



# Errors

The Smart-match API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request sucks
401 | Unauthorized -- Your API key is wrong
403 | Forbidden -- The requested resouce is hidden for administrators only
404 | Not Found -- The specified resouce could not be found
405 | Method Not Allowed -- You tried to access an endpoint with an invalid method
406 | Not Acceptable -- You requested a format that isn't json
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
