---
title: Cloakfusion API Reference

language_tabs:
  - python
  - shell


search: true
---

# Getting started

Welcome to the Cloakfusion API documentation! You can use our API to access API endpoints, which can help you create, update and delete properties. You can also retrieve information about the data usage of your properties.

We have language bindings in Shell, and Python. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

Our API uses an API token or your login session from our management portal. You can find your Cloakfusion API key at our [portal](https://my.cloakfusion.com).

For python examples we use requests library as http client to set up a session with the API.

## Glossary

Keyword | Definition
------- | -----------
CDN | A Content Delivery Network. We use multiple to guarantee 100% uptime
vhost / <br/>property | An entry in our multi-cdn solution for a domain name. E.g.: example.cdn.warpcache.com
origin | The url that contains files that will be pulled & cached by CDN's. E.g. http://o.example.com
domain name | The url which you can use to pull your files through the multi-cdn solution. E.g. http://cdn.example.com
flush | API representation of a flush/purge request to connected CDN providers to clear cache for an object
purge | See flush

## Formatting

API formatting can be manipulated through the format query parameter.

Our API supports multiple formatting types:

Parameter | Value | Default | Description
--------- | ----- | ------- | -----------
format | json | true | If set , the result will be returned as JSON.
format | api | false | If set , the result will be returned as HTML.
format | xml | false | If set , the result will be returned as XML.

## Errors

Cloakfusion uses conventional HTTP response codes to indicate success or failure of an API request. In general, codes in the 2xx range indicate success, codes in the 4xx range indicate an error that resulted from the provided information (e.g. a required parameter was missing, etc.), and codes in the 5xx range indicate an error with Cloakfusion's servers.

The Warpcache API has the following error codes:

Error Code | Meaning
---------- | -------
400 - Bad Request | Often missing a parameter
401 - Unauthorized | No valid API key provided
403 - Forbidden | The resource requested is unavailable for the API key provided
404 - Not Found | The specified resource doesn't exist
405 - Method Not Allowed | Check the  HTTP method
406 - Not Acceptable | The requested format isn't supported
500 - Internal Server Error | We had a problem with our server
503 - Service Unavailable | We're temporarially offline for maintanance

# Authentication

Authentication is managed via either cookies or an API token. As stated above, obtaining a token can be done by sending an email to support[at]cloakfusion.com. If you're logged in on our management portal, you don't have to do anything specific to access the API.

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
You should replace <code>YOUR_API_TOKEN</code> with your personal API key.
</aside>


# Vhosts

This endpoint allows you to retrieve, create and update information about vhosts.

## LIST

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

Example response:

```json
[ { "domain_name" : "example.cdn.warpcache.com",
    "id" : 1,
    "name" : "example",
    "origin_url" : "https://www.cloakfusion.com",
    "profile" : { "id" : 2,
        "name" : "Standard"
      },
    "properties" : [ { "cname" : "example.cdn.warpcache.com",
          "gzip" : true,
          "host_header" : "",
          "id" : 1,
          "override_hostheader" : false,
          "ssl" : true,
          "ttl" : 3600
        } ],
    "state" : "in_progress",
    "url" : "https://my.cloakfusion.com/api/v1/vhosts/1/"
  } ]
```

Returns a list of with vhost data objects.

`GET https://my.cloakfusion.com/api/v1/vhosts/`


## GET

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

vhost_id = 1
url = base_url + 'vhost/' + vhost_id
vhosts = api.get(url).json()
```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/vhosts/1" \
  -H "Authorization: YOUR_API_TOKEN"
```

> Example response:

```json
{ "domain_name" : "example.cdn.warpcache.com",
  "id" : 1,
  "name" : "example",
  "origin_url" : "https://www.cloakfusion.com",
  "profile" : { "id" : 2,
      "name" : "Standard"
    },
  "properties" : [ { "cname" : "example.cdn.warpcache.com",
        "gzip" : true,
        "host_header" : "",
        "id" : 1,
        "override_hostheader" : false,
        "ssl" : true,
        "ttl" : 3600
      } ],
  "state" : "in_progress",
  "url" : "https://my.cloakfusion.com/api/v1/vhosts/1/"
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
    'domain_name': 'another.example.com',
    'origin_url': 'http://o.example.com/',
    'name': 'another'
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

{ "domain_name" : "another.example.com",
  "id" : 2,
  "name" : "another",
  "origin_url" : "http://o.example.com",
  "profile" : { "id" : 2,
      "name" : "Standard"
    },
  "properties" : [  ],
  "state" : "in_progress",
  "url" : "https://my.cloakfusion.com/api/v1/vhosts/2/"
}

```

This method allows you to create vhosts.
<aside class="notice">
Note that vhost creation doesn't immediately return the CNAME record.<br/><br/>
It may take up to <b>30 seconds</b> before you will get a CNAME in the vhost response.


When a request for the vhost id(`2` in this case) doesn't yield a CNAME record, please contact support[at]cloakfusion.com and provide the vhost id, origin url and domain name.
</aside>

`POST https://my.cloakfusion.com/api/v1/vhosts/`

### Post Parameters

Parameter | Required | Description
--------- | -------- | -----------
domain_name | true | References the CDN vhost. This is the hostname the user would browse to to retrieve files.
name | true | Nice name by which user can identify the vhost (no functional purpose).
origin_url | true | URL that references the origin. Where the CDN will get the content fromContains protocol, domain name (fqdn), port (optional) and the full path.



# Vhostproperties


## LIST

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

url = base_url + 'vhostproperties/'
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
  { "cname" : "example.cdn.warpcache.com",
    "gzip" : true,
    "host_header" : "",
    "id" : 1,
    "override_hostheader" : false,
    "ssl" : true,
    "ttl" : 3600
  }
]
```

List of all vhost property objects.

`GET https://my.cloakfusion.com/api/v1/vhostproperties/`

## GET

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

vhost_property_id = 1
url = base_url + 'vhostproperties/' + vhost_property_id
new_vhost = api.get(url).json()
```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/vhostproperties/1" \
  -H "Authorization: YOUR_API_TOKEN" \
```

> Example response:

```json

{ "cname" : "example.cdn.warpcache.com",
  "gzip" : true,
  "host_header" : "",
  "id" : 1,
  "override_hostheader" : false,
  "ssl" : true,
  "ttl" : 3600
}

```

Show vhost property object for vhost id `id`.

`GET https://my.cloakfusion.com/api/v1/vhostproperties/` `id` /

## PATCH

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

vhost_property_id = 1
data = {
    'ttl': '300',
    'gzip': True
}
url = base_url + 'vhostproperties/' + vhost_property_id

vhost_property = api.put(url).json()
```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/vhostproperties/1" \
  -H "Authorization: YOUR_API_TOKEN" \
```

> Example response:

```json

{ "cname" : "example.cdn.warpcache.com",
  "gzip" : true,
  "host_header" : "",
  "id" : 1,
  "override_hostheader" : false,
  "ssl" : true,
  "ttl" : 300
}

```

This method allows you to update vhosts properties.

`PATCH https://my.cloakfusion.com/api/v1/vhostproperty/ ` `id` /

### Query Parameters

Parameter | Required | Description
--------- | -------- | -----------
cname | false | The hostname to which you link your domain e.g. foo.cdn.warpcache.com
ttl | false | Indicates the lifetime (in seconds) of the DNS record
gzip | false | Boolean. If enabled, the CDNs will be set to compress objects
ssl | false | Indicates whether a vhost is also available on SSL
override_hostheader | false | By default the Host header used to retrieve objects from origin is the origin hostname. Set to TRUE to use the name of the vhost in requests to origin
host_header | false | Allows you to specify the Host header to be used in requests to origin



# Flush


With this endpoint you can create a request to flush / purge an object from all connected cdn's.



<aside class="notice">You can flush the URLs that you're using on your properties. Given the following setup</aside>
 | |
 --- | ------
Domain name | **cdn.example.com**
Origin url | http://o.example.com |
Label | example
Cname | example.cdn.warpcache.com

Will let you flush on the domain name http:// **cdn.example.com** /


## LIST

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

url = base_url + 'flush/'
new_vhost = api.get(url).json()
```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/vhosts/" \
  -H "Authorization: YOUR_API_TOKEN" \
```

> Example response:

```json

{
  "count": 2,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": 2,
      "flushes": [
        2
      ],
      "urls": [
        "http://cdn.example.com/selfie.jpg",
        "http://cdn.example.com/team_photo.jpg"
      ],
      "owner": 1
    },
    {
      "id": 1,
      "flushes": [
        1
      ],
      "urls": [
        "http://cdn.example.com/selfie.jpg"
      ],
      "owner": 1
    },
]
```

List of all flushes from your organization.

`GET https://my.cloakfusion.com/api/v1/flush/`

## GET

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

flush_id = 1
my_flush = api.get(url+'flush/' + flush_id)

```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/flush/1" \
  -H "Authorization: YOUR_API_TOKEN" \
```

> Example response:

```json

{
  "id": 1,
  "flushes": [
    1
  ],
  "urls": [
    "http://example.cdn.warpcache.com/api.txt"
  ],
  "owner": 1
}

```

Show vhost property object for vhost id `id`.

`GET https://my.cloakfusion.com/api/v1/flush/` `id` /

## POST

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

import json
my_flush = api.post(url+'flush/', data=json.dumps(
    {
        'urls': [
            "http://example.cdn.warpcache.com/test.txt"
        ]
    }),
    headers={
        'Content-Type': 'application/json'
    }
)

```

```shell
# With shell, you can just pass the correct header with each request
curl "/api/v1/flush/1" \
  -H "Authorization: YOUR_API_TOKEN" \
```

> Example response:

```json
{
  "id": 1,
  "flushes": [
    1
  ],
  "urls": [
    "http://example.cdn.warpcache.com/api.txt"
  ],
  "owner": 1
}

```

This method allows you to create a flush request
<aside class="success">Don't forget to set the `Content-Type` header to `application/json`</aside>

`POST https://my.cloakfusion.com/api/v1/flush/`

### Post Parameters

Parameter | Required | Description
--------- | ------- | -----------
urls | true | The URL of the object you want to flush


# Flushstatus


Use this to retrieve the status of your flushes

## LIST

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

url = base_url + 'flushstatus/'
flushstatusses = api.get(url).json()

```

> Example response:

```json

[
  {
    "url": "https://my.cloakfusion.com/api/v1/flush/2/",
    "id": "https://my.cloakfusion.com/api/v1/flushstatus/2/",
    "flushrequest": 2,
    "error": "one or more cdns have failed to flush",
    "result": {
      "ready": true,
      "total": 2,
      "done": 2,
      "results": [
        [
          "CDN Provider",
          "flushed"
        ],
        [
          "CDN Provider 2",
          "flushed"
        ]
      ]
    },
    "state": "failed",
    "filenames": "[u'/test.txt', '/api.txt']",
    "created": "2015-07-13T08:49:03Z",
    "updated": "2015-07-13T08:49:04Z",
    "vhost": "https://my.cloakfusion.com/api/v1/vhosts/1/"
  },
  {
    "url": "https://my.cloakfusion.com/api/v1/flush/1/",
    "id": "https://my.cloakfusion.com/api/v1/flushstatus/1/",
    "flushrequest": 1,
    "error": "",
    "result": {
      "ready": true,
      "total": 1,
      "done": 2,
      "results": [
        [
          "CDN Provider",
          "flushed"
        ],
        [
          "CDN Provider 2",
          "flushed"
        ]
      ]
    },
    "state": "finished",
    "filenames": "[u'/test.txt']",
    "created": "2015-07-13T08:50:45Z",
    "updated": "2015-07-13T08:50:46Z",
    "vhost": "https://my.cloakfusion.com/api/v1/vhosts/1/"
  }
]

```

Retrieves a list of status/details for a flush request.

`GET https://my.cloakfusion.com/api/v1/flushstatus/`


## GET

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

url = base_url + 'flushstatus/2/'
flushstatus = api.get(url).json()

```

> Example response:

```json

{
  "url": "https://my.cloakfusion.com/api/v1/flush/2/",
  "id": "https://my.cloakfusion.com/api/v1/flushstatus/2/",
  "flushrequest": 2,
  "error": "one or more cdns have failed to flush",
  "result": {
    "ready": true,
    "total": 2,
    "done": 2,
    "results": [
      [
        "CDN Provider",
        "flushed"
      ],
      [
        "CDN Provider 2",
        "flushed"
      ]
    ]
  },
  "state": "failed",
  "filenames": "[u'/test.txt', '/api.txt']",
  "created": "2015-07-13T08:49:03Z",
  "updated": "2015-07-13T08:49:04Z",
  "vhost": "https://my.cloakfusion.com/api/v1/vhosts/1/"
}

```

Retrieves a single status/detail report for a flush request.

`GET https://my.cloakfusion.com/api/v1/flushstatus/`


# Traffic


Get information on traffic usage

## LIST

```python

""" Assuming we keep our api and base_url
variables from the authentication example: """

url = base_url + 'traffic/'
traffic = api.get(url).json()

```

> Example response:

```json

[
    {
        "usage": "1262.93",
        "quantity": "gb",
        "text": "1262.93 gb",
        "vhost": {
            "domain_name": "example.cdn.warpcache.com",
            "id": 1,
            "name": "example",
            "origin_url": "http://o.cloakfusion.com",
            "state": "active",
            "url": "https://my.cloakfusion.com/api/v1/vhosts/1/",
            "properties": [],
            "profile": {
                "name": "Standard",
                "id": 2
            }
        }
    },
    {
        "usage": "1262.93",
        "quantity": "gb",
        "text": "1262.93 gb",
        "vhost": {
            "domain_name": "cdn.example.com",
            "id": 2,
            "name": "mycompany",
            "origin_url": "http://o.example.com",
            "state": "active",
            "url": "https://my.cloakfusion.com/api/v1/vhosts/2/",
            "properties": [],
            "profile": {
                "name": "Standard",
                "id": 2
            }
        }
    }
]

```

Lists all usage for all vhosts

`GET https://my.cloakfusion.com/api/v1/traffic/`

## GET

```python

{
    "usage": "1262.93",
    "quantity": "gb",
    "text": "1262.93 gb",
    "vhost": {
        "domain_name": "example.cdn.warpcache.com",
        "id": 1,
        "name": "example",
        "origin_url": "http://o.cloakfusion.com",
        "state": "active",
        "url": "https://my.cloakfusion.com/api/v1/vhosts/1/",
        "properties": [],
        "profile": {
            "name": "Standard",
            "id": 2
        }
    }
}

```

Retrieve usage for one vhost

`GET https://my.cloakfusion.com/api/v1/traffic/` `id` /

<aside class="success">
Where id = vhost id
</aside>

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
start_date | false | Start date for fetching traffic usage. Format: dd-mm-yyyy
end_date | false | End date for fetching traffic usage. Format: dd-mm-yyyy

