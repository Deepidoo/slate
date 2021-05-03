---
title: Deepidoo API Version 2 Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - json

toc_footers:
  - <a href='https://github.com/lord/slate' target='_blank'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Deepidoo API version 2. This RESTfull / SSL based API is based on JSON and uses standard OAuth2 for authentication. It is fully compatible for any usage straight from a mobile device, and can also be used for exchanging data from server to server.

_In case you used the legacy API version 1, you should only have to change the base endpoint (and enforce the use of SSL) to make it work._


__Base endpoint URL__ 

`https://play.deepidoo.com/api/v2`

- All paths in this documentation are __relative__ to this endpoint, do not forget to prefix
your requests in case you'd copy/paste CURL examples.
- All parameter names are case sentitive (eg: User_Id and user_id are __not__ the same).
- All path segment using ":" refers to a parameter (typically for GET requests, eg: "/boxes/:id" means you have to pass the ID of the box in the URL)


# Authentication

> CURL request / JSON response example:

```shell
curl  -XPOST
      -H "Content-type: application/json"
      -d '{
      "client_id": "API_CLIENT_ID",
      "client_secret": "API_CLIENT_SECRET",
      "username":"user@domain.com",
      "password": "password123"
    }' 
    "/oauth/token"
```

```json
{
  "access_token": "some64charstring",
  "token_type": "bearer",
  "expires_in": 86400, // in seconds
  "created_at": 1549535525 // Unix epoch 
}
```

Deepidoo API uses OAuth2 to allow access to the API. You can create a new session using those parameters:

### Create a new session

`POST /oauth/token`

Parameter | Type | Description
--------- | ------- | -----------
username* | String | Email of the user 
password* | String | Password of the user  
client_id* | String | API_CLIENT_ID 
client_secret* | String | API_CLIENT_SECRET

<aside class="warning">
<b>*</b> Required parameters
</aside>

### Using the token through the session

Once you provided valid credentials, you will be given a token. This token is valid for 2 Days.
You can now perform calls passing the token along with your request, either in the query string
(deprecated) or the "Authorization" HTTP header (prefered), as documented [here](https://tools.ietf.org/html/rfc6750#section-2.1).

<aside class="notice">
Session tokens are valid for 2 days (86400 seconds), so you can store and reuse them to avoid useless calls. <br />
<b>It is strongly recommended to renew session tokens only once a day</b>
</aside>

# Events

## Get all registered events

> CURL request / JSON response examples:

```shell
curl -XGET
     -H "Content-type: application/json"
     -H "Authorization: Bearer 64-CHAR_SESSION_TOKEN"
        "/events"

# OR using the query string for authentification

curl -XGET 
     -H "Content-type: application/json"
        "/events?access_token=64-CHAR_SESSION_TOKEN"
```

```json
{
  "events": [
    {
      "id": "one-1234", 
      "name": "first, audio event title", 
      "min_threshold": 0, 
      "max_threshold": 1000000, 
      "event_type": "audio", 
      "duration": 0, 
      "interleave": 0, 
      "loop": false, 
      "real_time": true, 
      "is_active": true
    }, {
      "id": "two-5678",
      "name": "second, audio event title", 
      "min_threshold": 1000, 
      "max_threshold": 1000000, 
      "event_type": "audio", 
      "duration": 0, 
      "interleave": 0, 
      "loop": false, 
      "real_time": false, 
      "is_active": true
    }
    ...
  ]
}
```

`GET /events`

## Get event details

> CURL request / JSON response examples:

```shell
curl -XGET
     -H "Content-type: application/json"
     -H "Authorization: Bearer 64-CHAR_SESSION_TOKEN"
        "/events/user_defined_key"
```

```json
{
  "event": {
    "id": "12345", 
    "name": "second, audio event title", 
    "min_threshold": 1000, 
    "max_threshold": 1000000, 
    "event_type": "audio", 
    "duration": 0, 
    "interleave": 0, 
    "loop": false, 
    "real_time": false, 
    "is_active": true, 
    "exclusive_mode": false, 
    "contents": [
      {
        "id": 4, 
        "duration": 10, 
        "name": "audio_1", 
        "type": "audio_ad"
      }
    ], 
    "shops": [
      {"id": 1, "name": "shop 1"}, 
      {"id": 2, "name": "shop 2"}
    ]
  }
}
```

`GET /events/:id`

## Start an event 

> CURL request / JSON response examples:

```shell
curl -XPOST
     -H "Content-type: application/json"
     -H "Authorization: Bearer 64-CHAR_SESSION_TOKEN"
        "/events/user_defined_key"
```

```json
{
  "success": true,
  "errors": []
}
```

`POST /events/:id`

Parameter | Type | Description
--------- | ------- | -----------
id* | Integer | event ID  
value | Integer | Value to be evaluated in case the event use thresholds<br /><i>Note:</i>Default value is 0
origin_id | Integer | ID of the shop that originated the event. <br /><i>Note:</i> required if you use the "exclusive mode" option, otherwise, this option will be silently ignored

<aside class="warning">
<b>*</b> Required parameter
</aside>

## Stop an event 

> CURL request / JSON response examples:

```shell
curl -XDELETE
     -H "Content-type: application/json"
     -H "Authorization: Bearer 64-CHAR_SESSION_TOKEN"
        "/events/user_defined_key"
```

```json
{
  "success": true,
  "errors": []
}
```

`DELETE /events/:id`

Parameter | Type | Description
--------- | ------- | -----------
id* | Integer | event ID 

<aside class="warning">
<b>*</b> Required parameter
</aside>

# Boxes

## Get Box details

> CURL request / JSON response examples:

```shell
curl -XGET
     -H "Content-type: application/json"
     -H "Authorization: Bearer 64-CHAR_SESSION_TOKEN"
        "/boxes/1234"

# OR using the query string for authentification

curl -XGET 
     -H "Content-type: application/json"
        "/boxes/1234?access_token=64-CHAR_SESSION_TOKEN"
```

```json
{
  "box": {
    "box_type": "audio", 
    "ref": 1, 
    "active": true, 
    "box_tag": {
      "label": "Box tag example", 
      "description": "Tag description", 
      "color": "#167c31"
    }
  }
}
```

`GET /boxes/:id`

Parameter | Type | Description
--------- | ------- | -----------
id* | Integer | ID of the device 

<aside class="warning">
<b>*</b> Required parameter
</aside>

## Trigger a Live event on a Box

> CURL request / JSON response examples:

```shell
curl  -XPOST 
      -H "Content-type: application/json"
      -H 'Authorization: Bearer 64-CHAR_SESSION_TOKEN'
      -d '{"medias": [
          {
            "deepiref": "deepi-img_201603161025_ef8d4d2ba87359463ab97552826427182e1700d4",
            "deepiref_ext": "my_own_ref",
            "duration": 60
          },
          {
            "source_id": "31190",
            "duration": 60
          }
        ]
      }' 
      '/boxes/1234/start_live'
```

```json
{
  "information":{
    "message": "200 OK !",
    "code": 200
  }
}
```

`POST /boxes/:id/start_live`

Parameter | Type | Description
--------- | ------- | -----------
id* | Integer | ID of the device
medias* | Array | Array of Medias as JSON Objects

<aside class="warning">
<b>*</b> Required parameters
</aside>

## Stop a Live event on a Box

> CURL request / JSON response examples:

```shell
curl  -XPOST 
      -H "Content-type: application/json"
      -H 'Authorization: Bearer 64-CHAR_SESSION_TOKEN'
      '/boxes/1234/stop_live'
```

```json
{
  "information":{
    "message": "200 OK !",
    "code": 200
  }
}
```

`POST /boxes/:id/stop_live`

Parameter | Type | Description
--------- | ------- | -----------
id | Integer | ID of the device

<aside class="warning">
<b>*</b> Required parameter
</aside>

# Shops 

## List all shops for a given user

> CURL request / JSON response examples:

```shell
curl  -XGET 
      -H "Content-type: application/json"
      -H 'Authorization: Bearer 64-CHAR_SESSION_TOKEN'
      '/shops'
```

```json
{
  "shops": [
    {
      "title": "My first shop",
      "additional_ref": "00042",
      "boxes": [
        {
          "box_type": "audio",
          "ref": 231,
          "active": true,
          "is_alive": true,
          "last_response_at": "2019-04-18T14:32:09.000+02:00",
          "box_tag": {
            "label": "window",
            "description": "Awesome box tag",
            "color": "#61cdc9"
          }
        },{
          "box_type": "audio",
          "ref": 233,
          "active": true,
          "is_alive": false,
          "last_response_at": "2019-01-11T04:22:00.000+02:00",
          "box_tag": {
            "label": "window",
            "description": "Awesome box tag",
            "color": "#61cdc9"
          }
        }
      ]
    }
  ]
}
```

`GET /shops`

## List all thresholds for a given shop

> CURL request / JSON response examples:

```shell
curl  -XGET 
      -H "Content-type: application/json"
      -H 'Authorization: Bearer 64-CHAR_SESSION_TOKEN'
      '/shops/:id/thresholds'
```

```json
{
  "thresholds": [
    {
      "id": 2, 
      "threshold": 1000, 
      "shop_id": 1, 
      "event_title": "An event title"
    }
  ]
}
```

`GET /shops/:id/thresholds`

Parameter | Type | Description
--------- | ------- | -----------
id* | String | Reference to a shop
order | String | Must of ['desc', 'asc'], order of the list
limit | Int | Number of thresholds to display

<aside class="warning">
<b>*</b> Required parameters
</aside>

## Start a live event according to a threshold

> CURL request / JSON response examples:

```shell
curl  -XPOST 
      -H "Content-type: application/json"
      -H 'Authorization: Bearer 64-CHAR_SESSION_TOKEN'
      -d '{"amount": 1000.50, "options": {"slot": 1, "message": "hello world"}}' 
      '/shops/1234/trigger'
```

```json
{
  "information":{"message":"200 OK !","code":200}
}
```

`POST /shops/:id/trigger`

Parameter | Type | Description
--------- | ------- | -----------
id* | String | Reference to a shop
amount* | Double | Amount to be compared against parameterized thresholds
delayed | Boolean | If set to false, event will be played right now. Otherwise, will wait for current media to end first, before showing up the live event. Default is 'false'
loop | Boolean | If set to true, events will be played in loop mode, until a close event is triggered. If set to false, will be played only once. Default is 'false'
event_type| String | Must be one of ['audio', 'video', 'all']. Default to 'all'
options | Object | Options in {"key": value} format for this event, will be forwarded to the Device

<aside class="warning">
<b>*</b> Required parameters
</aside>

## Stop running live events

> CURL request / JSON response examples:

```shell
curl  -XPUT 
      -H "Content-type: application/json"
      -H 'Authorization: Bearer 64-CHAR_SESSION_TOKEN'
      '/shops/1234/close_events'
```

```json
{
  "information":{"message":"200 OK !","code":200}
}
```

`PUT /shops/:id/close_events`

Parameter | Type | Description
--------- | ------- | -----------
id* | String | Reference to a shop
event_type | String | Must be one of ['audio', 'video', 'all']. Default to 'all'

<aside class="warning">
<b>*</b> Required parameters
</aside>

# Groups

## List event thresholds

> CURL request / JSON response examples:

```shell
curl -XGET
     -H "Content-type: application/json"
     -H "Authorization: Bearer 64-CHAR_SESSION_TOKEN"
        "/groups/thresholds"
```

```json
{
  "thresholds": [
    {
      "threshold": 1000, 
      "title": "first event title", 
      "eventable": [
        {"group_id": 1}
      ]
    }, 
    {
      "threshold": 1000, 
      "title": "second event title", 
      "eventable": [
        {"group_id": 1}
      ]
    }
  ]
}
```

`GET /groups/thresholds`

Parameter | Type | Description
--------- | ------- | -----------
order | String | one of "asc" or "desc": order of the listing (default: 'desc')
limit | Integer | Number of thresholds

## List available medias

> CURL request / JSON response examples:

```shell
curl -XGET
     -H "Content-type: application/json"
     -H "Authorization: Bearer 64-CHAR_SESSION_TOKEN"
        "/groups/list_medias"
```

```json
{
  "medias": [
    {
      "media": {
        "deepiref": null, 
        "deepiref_ext": null, 
        "media_type": "video", 
        "on_local": true, 
        "references": [
          {"label": "video_one_1"}
        ]
      }
    }, {
      "media": {
        "deepiref": null, 
        "deepiref_ext": null, 
        "media_type": "video", 
        "on_local": true, 
        "references": [
          {"label": "video_two_2"}
        ]
      }
    }
  ]
}
```

`GET /groups/list_medias`

Parameter | Type | Description
--------- | ------- | -----------
type | String | one of ["music", "audio_ad", "video_ad", "image", "displayable"]


## Trigger an event

> CURL request / JSON response examples:

```shell
curl  -XPOST 
      -H "Content-type: application/json"
      -H 'Authorization: Bearer 64-CHAR_SESSION_TOKEN'
      -d '{"shop_id": 1234, "amount": 1000, "event_type": "audio"}' 
      '/groups/trigger'
```

```json
{
  "information":{
    "message": "200 OK !",
    "code": 200
  }
}
```

`POST /groups/trigger`

Parameter | Type | Description
--------- | ------- | -----------
shop_id* | Integer | Id of the shop triggering the event
amount*  | Integer | amount relative to the event that should be started
event_type*| String| Must be one of ["audio", "video", "all"]. Default: "all" 

<aside class="warning">
<b>*</b> Required parameters
</aside>

# Playlists 

## Get all playlists for a user

> CURL request / JSON response examples:

```shell
curl  -XGET 
      -H "Content-type: application/json"
      -H 'Authorization: Bearer 64-CHAR_SESSION_TOKEN'
      -d '{"managed_user_email": "user@email.com"}' 
      '/media/mixes'
```

```json
{
  "mixes": [
    {
      "id": 1, 
      "label": "streaming audio playlist 1", 
      "serialized_label": "1st label", 
      "color": "#ff7E0E", 
      "musics": [
        {
          "id": 6,
          "title": "music_1", 
          "color": "#d01cb1", 
          "artists": [
            {"name": "madonna"}
          ]
        }, 
        {
          "id": 7,
          "title": "music_2", 
          "color": "#dff921", 
          "artists": []
        }, 
      ],
      "tags": [
        {
          "title": "media_tag label", 
          "serialized_label": "media_tag serialized_label", 
          "color": "#02c595", 
          "description": "media_tag description"
        }
      ]
    }
  ]
}
```

`GET /media/mixes`

Parameter | Type | Description
--------- | ------- | -----------
managed_user_email | String | Email of the managed user owning the playlist

