---
title: Cloakfusion API Reference

language_tabs:
  - python
  - shell


includes:
  - errors

search: true
---

# Getting started

Welcome to the Cloakfusion API documentation! You can use our API to access API endpoints, which can help you create, update and delete [properties](#glossary). You can also retrieve information about the data usage of your properties.

We have language bindings in Shell, and Python. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

Our API uses an API token or your login session from our management portal. You can find your Cloakfusion API key at our [portal](https://my.cloakfusion.com).

For python examples we use requests library as http client to set up a session with the API.

### Glossary

Keyword | Definition
------- | -----------
CDN | A Content Delivery Network. We use multiple to guarantee 100% uptime
property | A 'vhost' for a domain name. E.g.: static.cdn.warpcache.com
vhost | An entry in our multi-cdn solution for a domain name. E.g.: static.cdn.warpcache.com
origin | The url that contains files that will be pulled & cached by CDN's. E.g. http://o.mysite.com
domain name | The url which you can use to pull your files through the multi-cdn solution. E.g. http://cdn.mysite.com

### Formatting

Our API supports multiple formatting types:

Parameter | Default | Description
--------- | ------- | -----------
format=json | true | If set , the result will be returned as JSON.
format=api | false | If set , the result will be returned as HTML.
format=xml | false | If set , the result will be returned as XML.



# Authentication

Authentication is managed via either cookies or an API token. As stated above, obtaining a token can be done by sending an email to tech_contact[at]cloakfusion.com. If you're logged in on our management portal, you don't have to do anything specific to access the API.

```python

import requests

token = 'YOUR_API_TOKEN'
api = requests.session()
api.headers.update({'Authorization': "Token %s" % token})
base_url = 'https://my.cloakfusion.com/api/v1/'

print api.get(base_url).json()

```

```shell
# With shell, you can just pass the correct header with each request
curl -X GET https://my.cloakfusion.com/api/v1/ -H 'Authorization: Token YOUR_API_TOKEN'
```

> Make sure to replace `YOUR_API_TOKEN` with the real token.

For clients to authenticate, the token key should be included in the Authorization HTTP header. The key should be prefixed by the string literal "Token", with whitespace separating the two strings.
You can get your Cloakfusion API key at our [portal](https://my.cloakfusion.com).

<aside class="notice">
You should replace <code>YOUR_API_TOKEN</code> with your personal API key of course.
</aside>


# Vhosts

This endpoint allows you to retrieve, create and update information about vhosts.

## GET

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

url = base_url + 'vhost/'
vhosts = api.get(url).json()
```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/vhosts/" \
  -H "Authorization: YOUR_API_TOKEN"
```

> Example response:

```json
[ { "domain_name" : "static-cloakfusion.cdn.warpcache.com",
    "id" : 1,
    "name" : "static_cloak",
    "origin_url" : "https://www.cloakfusion.com",
    "profile" : { "id" : 2,
        "name" : "Standard"
      },
    "properties" : [ { "cname" : "static-cloakfusion.cdn.warpcache.com",
          "gzip" : true,
          "host_header" : "",
          "id" : 1,
          "override_hostheader" : false,
          "ssl" : true,
          "ttl" : 3600
        } ],
    "state" : "in_progress",
    "url" : "http://my.cloakfusion.com/api/v1/vhosts/1/"
  } ]
```

Returns a list of with vhost data objects.

`GET https://my.cloakfusion.com/api/v1/vhosts/`


## GET One

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

vhost_id = 1
url = base_url + 'vhost_property/' + vhost_id
url = base_url + 'vhost/'
vhosts = api.get(url).json()
```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/vhosts/1" \
  -H "Authorization: YOUR_API_TOKEN"
```

> Example response:

```json
{ "domain_name" : "static-cloakfusion.cdn.warpcache.com",
  "id" : 1,
  "name" : "static_cloak",
  "origin_url" : "https://www.cloakfusion.com",
  "profile" : { "id" : 2,
      "name" : "Standard"
    },
  "properties" : [ { "cname" : "static-cloakfusion.cdn.warpcache.com",
        "gzip" : true,
        "host_header" : "",
        "id" : 1,
        "override_hostheader" : false,
        "ssl" : true,
        "ttl" : 3600
      } ],
  "state" : "in_progress",
  "url" : "http://my.cloakfusion.com/api/v1/vhosts/1/"
}
```

Returns a list of with vhost data objects.

`GET https://my.cloakfusion.com/api/v1/vhosts/`


## POST

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

url = base_url + 'vhost/'

data = {
    'domain_name': 'apidocs.cloakfusion.com',
    'origin_url': 'https://storage.googleapis.com/api_docs/index.html',
    'name': 'api'
}
new_vhost = api.post(url, data=data).json()
```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/vhosts/" \
  -H "Authorization: YOUR_API_TOKEN" \
  --data ""
```

> Example response:

```json

{ "domain_name" : "apidocs.cloakfusion.com",
  "id" : 2,
  "name" : "api",
  "origin_url" : "https://storage.googleapis.com/api_docs/index.html",
  "profile" : { "id" : 2,
      "name" : "Standard"
    },
  "properties" : [  ],
  "state" : "in_progress",
  "url" : "http://my.cloakfusion.com/api/v1/vhosts/2/"
}

```

This method allows you to create vhosts.
<aside class="notice">
Note that vhost creation doesn't immediately return the cname record.<br/><br/>
It may take up to <b>30 seconds</b> before you will get a cname in the vhost response.


When a request for the vhost id(`2` in this case) doesn't yield a cname record, please contact tech_contact[at]cloakfusion.com and provide the vhost id, origin url and domain name.
</aside>

`POST https://my.cloakfusion.com/api/v1/vhosts/`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
domain_name | true | References the CDN vhost. This is the hostname the user would browse to to retrieve files.
name | true | Nice name by which user can identify the vhost (no functional purpose).
origin_url | true | URL that references the origin. Where the CDN will get the content fromContains protocol, domain name (fqdn), port (optional) and the full path.



# Vhost property


## GET

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

url = base_url + 'vhost_property/'
new_vhost = api.get(url).json()
```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/vhosts/" \
  -H "Authorization: YOUR_API_TOKEN" \
```

> Example response:

```json

[ { "cname" : "api.cdn.warpcache.com",
    "gzip" : false,
    "host_header" : "",
    "id" : 2,
    "override_hostheader" : false,
    "ssl" : false,
    "ttl" : 300
  },
  { "cname" : "static-cloakfusion.cdn.warpcache.com",
    "gzip" : true,
    "host_header" : "",
    "id" : 1,
    "override_hostheader" : false,
    "ssl" : true,
    "ttl" : 3600
  }
]
```

Lists of all vhost property objects.

`GET https://my.cloakfusion.com/api/v1/vhost_property/`

## GET One

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

vhost_property_id = 1
url = base_url + 'vhost_property/' + vhost_property_id
new_vhost = api.get(url).json()
```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/vhost_property/1" \
  -H "Authorization: YOUR_API_TOKEN" \
```

> Example response:

```json

{ "cname" : "static-cloakfusion.cdn.warpcache.com",
  "gzip" : true,
  "host_header" : "",
  "id" : 1,
  "override_hostheader" : false,
  "ssl" : true,
  "ttl" : 3600
}

```

Show vhost property object for vhost id `id`.

`GET https://my.cloakfusion.com/api/v1/vhost_property/` `id` /

## PUT

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

vhost_property_id = 1
data = {
    'ttl': '300',
    'gzip': True
}
url = base_url + 'vhost_property/' + vhost_property_id

vhost_property = api.put(url).json()
```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/vhost_property/1" \
  -H "Authorization: YOUR_API_TOKEN" \
```

> Example response:

```json

{ "cname" : "static-cloakfusion.cdn.warpcache.com",
  "gzip" : true,
  "host_header" : "",
  "id" : 1,
  "override_hostheader" : false,
  "ssl" : true,
  "ttl" : 300
}

```

This method allows you to update vhosts properties.

`PUT https://my.cloakfusion.com/api/v1/vhost_property/` `id` /

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
cname | false | The hostname to which you link your domain e.g. foo.cdn.warpcache.com
ttl | false | Indicates the lifetime (in seconds) of the DNS record
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

With this endpoint you can create a request to flush or purge an object from all connected cdn's.

## POST

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

api = Cloakfusion::APIClient.authorize!('meowmeowmeow')
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
ID | The ID of the kitten to retrieve

