---
title: API Reference

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

Welcome to the Deepidoo API version 2. This REST / SSL based -stateless- API is based on JSON and uses standard OAuth2 for authentication. It is fully compatible for any usage straight from a mobile device, and can also be used for exchanging data from server to server.

In case you used the legacy API version 1, you should only have to change the base endpoint (and enforce the use of SSL) to make it work.


__Base endpoint URL__ 

`https://play.deepidoo.com/api/v2`

- All paths in this documentation are __relative__ to this endpoint, do not forget to prefix
your requests in case you'd copy/paste CURL examples.
- All parameter names are case sentitive (eg: User_Id and user_id are __not__ the same).
- All path segment using ":" refers to a parameter (typically for GET requests, eg: "/boxes/:id" means you have to pass the ID of the box in the URL)


# Authentication

> Click on the JSON tab to get the JSON response of this CURL example:

```shell
curl "/oauth/token"
  -H "Authorization: meowmeowmeow"
```

```json
{
  "access_token": "some64charstring",
  "token_type": "bearer",
  "expires_in": 7200, // in seconds
  "created_at": 1549535525 // Unix epoch 
}
```

Deepidoo API uses OAuth2 to allow access to the API. You can create a new session using those parameters:

### Create a new session

`POST /oauth/token`

Parameter | Type | Description
--------- | ------- | -----------
username | String | URL encoded Email of the user 
password | String | Password of the user  
client_id | String | API_CLIENT_ID 
client_secret | String | API_CLIENT_SECRET

<aside class="notice">
Replace <code>API_CLIENT_ID and API_CLIENT_SECRET</code> with your personal API credentials provided by Deepidoo.
</aside>

### Using the token through the session

Once you provided valid credentials, you will be given a token. This token is valid for 2 hours.
You can now perform calls passing the token along with your request, either in the query string
(deprecated) or the "Authorization" HTTP header (prefered), as documented [here](https://tools.ietf.org/html/rfc6750#section-2.1).

# Boxes

### Get Box informations

> Click on the JSON tab to get the JSON response of this CURL example:

```shell
curl "/boxes/1234"
  -H "Authorization: Bearer 0b79bab50daca910b000d4f1a2b675d604257e42"
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
id | Integer | ID of the device 





