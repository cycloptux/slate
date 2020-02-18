---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the DisComm Bots API! You can use our API to access DisComm Bots API endpoints, which can get information on each one of our bots and provide alternative ways of setting up your Discord modules.

For any given API endpoint, you can view Shell requests examples in the dark area to the right of this page.

# Authentication

> To send an authorized request, use this code:

```shell
curl "api_endpoint_here"
  --header "Authorization: abcdefghijklmnopqrstuvwxyz0123456789"
```

```javascript
fetch("api_endpoint_here", { "headers": {
    "Authorization: abcdefghijklmnopqrstuvwxyz0123456789"
} })
  .then(response => response.json())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

> Make sure to replace `abcdefghijklmnopqrstuvwxyz0123456789` with your access token.

DisComm Bots API endpoints are accessed through the use of an API Gateway. The API Gateway uses your Discord user info to match with your Discord account, show your authorized endpoints and fetch the info you need from the Discord API.

Throughout the login & authorization workflow, a callback to the native dashboard is implemented in order to complete the login workflow. If you are interested in building an alternative client, contact us to register your callback URL.

The API Gateway expects for the received access token to be included in all API requests to the server in a header that looks like the following:

`Authorization: abcdefghijklmnopqrstuvwxyz0123456789`

<aside class="notice">
You must replace <code>abcdefghijklmnopqrstuvwxyz0123456789</code> with your personal access token.
</aside>

The API Gateway also accepts the following headers:

`Authorization: Bearer abcdefghijklmnopqrstuvwxyz0123456789`

`x-access-token: abcdefghijklmnopqrstuvwxyz0123456789`

`x-access-token: Bearer abcdefghijklmnopqrstuvwxyz0123456789`

## Obtaining an Access Token

In order to obtain an API Gateway access token, users first need to authenticate themselves with their Discord account.

The gateway will obtain the user's Discord identity, including the Discord access token. The Discord token will be retained by the API gateway and will be hidden to the front-end, enhancing security for the end-user.

Obtaining the access token to the API gateway is a 2-step process.

### Authorization: Step 1

First of all, generate a new Discord login request by hitting the following gateway endpoint:

#### HTTP Request

`GET https://gateway.cycloptux.com/auth/discord/login?redirect_uri=<CALLBACK_URL>`

#### HTTP Response

`Found. Redirecting to https://discordapp.com/api/oauth2/authorize?client_id=<APP_CLIENT_ID>&scope=identify&response_type=code&state=<RANDOM_CODE>&redirect_uri=https%3A%2F%2Fgateway.cycloptux.com%2Fauth%2Fdiscord%2Fcallback`

The user must then login through the provided redirect. Upon a successful login, the callback URL will contain a new query string parameter, `?c=<TEMPORARY_TOKEN>`, that will be needed for the second login phase.

### Authorization: Step 2

The temporary short token must then be exchanged with a full token in order to send further requests to the API gateway:

#### HTTP Request

```shell
curl --request GET 'https://gateway.cycloptux.com/auth/discord/token' \
  --header 'Authorization: Bearer <TEMPORARY_TOKEN>'
```

```javascript
fetch("https://gateway.cycloptux.com/auth/discord/token", { "headers": {
    "Authorization: Bearer <TEMPORARY_TOKEN>"
} })
  .then(response => response.json())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

`GET https://gateway.cycloptux.com/auth/discord/token`

Using header:

`Authorization: Bearer <TEMPORARY_TOKEN>`

#### HTTP Response

> The long access token will only be shown in this request and will not be obtained through subsequent requests. Make sure to save it.

```json
{
    "access_token": <LONG_TOKEN>,
    "is_valid": true,
    "issued_at": <ISSUED_DATE>,
    "expiration_date": <EXPIRATION_DATE>,
    "message": "Authorization successful."
}
```

The long `access_token` provided here must be added to all requests in order to identify the user and its authorization set.



# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

