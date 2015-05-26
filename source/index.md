---
title: Cloakfusion API Reference

language_tabs:
  - shell
  - python
  - php

includes:
  - errors

search: true
---

# Introduction!!!!

Welcome to the Cloakfusion API documentation! You can use our API to access Cloakfusion API endpoints, which can help you create, update and delete properties. You can also retrieve information about the data usage of your properties.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

### Formatting

Parameter | Default | Description
--------- | ------- | -----------
format=json | true | If set , the result will be returned as JSON.
format=api | false | If set , the result will be returned as HTML.

# Authentication

> To authorize, use this code:

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl -X GET https://my.cloakfusion.com/api/v1/vhosts/ -H 'Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b'
```

> Make sure to replace `9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b` with the real token.

For clients to authenticate, the token key should be included in the Authorization HTTP header. The key should be prefixed by the string literal "Token", with whitespace separating the two strings.
You can register a new Cloakfusion API key at our [portal](https://my.cloakfusion.com).

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>


# Vhost

> To authorize, use this code:


```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

This endpoint allows you to retrieve information about vhosts.

### GET

This method lists all vhosts.

`GET https://my.cloakfusion.com/api/v1/vhosts/`


### POST

This method allows you to create and update? vhosts.

`POST https://my.cloakfusion.com/api/v1/vhosts/`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
domain_name | true | References the CDN vhost. This is the hostname the user would browse to to retrieve files.
name | true | Nice name by which user can identify the vhost (no functional purpose).
origin_url | true | URL that references the origin. Where the CDN will get the content fromContains protocol, domain name (fqdn), port (optional) and the full path.



# Vhostproperty

> To authorize, use this code:


```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

This endpoint allows you to retrieve and modify specific vhost properties.

### GET

This method lists all vhost properties.

`GET https://my.cloakfusion.com/api/v1/vhostsproperty/`

### POST

This method allows you to create and update? vhosts.

`POST https://my.cloakfusion.com/api/v1/vhosts/`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
cname | false | The hostname to which you link your domain e.g. foo.cdn.warpcache.com
ttl | true | Indicates the lifetime (in seconds) of the DNS record
gzip | false | Boolean. If enabled, the CDNs will be set to compress objects
ssl | false | Indicates whether a vhost is also available on SSL
override_hostheader | false | By default the Host header used to retrieve objects from origin is the origin hostname. Set to TRUE to use the name of the vhost in requests to origin
host_header | false | Allows you to specify the Host header to be used in requests to origin


# Flush

> To authorize, use this code:


```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

With this endpoint you can create a request to flush or purge an object.

### POST

This method allows you to create a purge request

`POST https://my.cloakfusion.com/api/v1/flush/`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
urls | true | The URL of the object you want to flush


# Flushstatus

> To authorize, use this code:


```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Use this to retrieve the status of your flushes

### POST

This method allows you to request the status of a purge request

`POST https://my.cloakfusion.com/api/v1/flushstatus/`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
filenames | true | The names of the objects (regex)
vhosts | true | The vhost you want to retrieve the information for



# Traffic

> To authorize, use this code:

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Get information on the traffic

### POST

This method allows you to request the status of a purge request

`GET https://my.cloakfusion.com/api/v1/traffic/`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
foo | bar | foobar



# Examples

## Get All Kittens

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
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
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/kittens`

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

api = Cloakfusion::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/3"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the cat to retrieve

