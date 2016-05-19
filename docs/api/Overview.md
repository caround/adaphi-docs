This page describes the resources that make up the Adaphi API **v1**.

[TOC]

## Current Version
By default, requests without `Accept` header receive the **v1** version of the API. We encourage you to explicitly request version via the `Accept` header.

```
Accept: application/vnd.adaphi.v1+json
```
## Schema
All API access is over HTTPS and accessed from `https://api.adaphi.com`.

All data is sent and received as JSON.

All timestamps are returned in ISO 8601 format:

```
YYYY-MM-DDThh:mm:ssZ
```
Blank fields are included as `null` instead of being omitted.

### Summary Representations
When you fetch a list of resources, the response includes a subset of the attributes for that resource. This is the "summary" representation of the resource. (Some attributes are computationally expensive for the API to provide. For performance reasons, the summary representation excludes those attributes. To obtain those attributes, fetch the "detailed" representation.)

### Detailed Representations
When you fetch an individual resource, the response typically includes all attributes for that resource. This is the "detailed" representation of the resource. (Note that authorization sometimes influences the amount of detail included in the representation.)

## Parameters
Many API methods take optional parameters. For `GET` requests, any parameters not specified as a segment in the path can be passed as an HTTP query string parameter:

```
curl -i "https://api.adaphi.com/resources?filter=value"
```

For `POST`, `PATCH`, `PUT`, and `DELETE` requests, parameters not included in the URL should be encoded as JSON with a `Content-Type` of 'application/json'.

## Root Endpoint
Issue a `GET` request to the root endpoint will get all the endpoint categories that the API supports:

```
curl https://api.adaphi.com
```

## Client Errors
There are three possible types of client errors on API calls that receive request bodies:

1. Sending invalid JSON will result in a `400 Bad Request` response
2. Sending the wrong type of JSON values will result in a `400 Bad Request` response.
3. Sending invalid fields will result in a `422 Unprocessable Entity` response.

All error objects have resource and field properties so that your client can tell what the problem is. There's also an error code to let you know what is wrong with the field. These are the possible validation error codes:

|Error Nmae				|Description		|
|:-------------			|:---------------|
|missing					|This means a resource does not exist.|
|missing_field			|This means a required field on a resource has not been set.  |
|invalid					|This means the formatting of a field is invalid.    |
|already_exists			|This means another resource has the same value as this field. This can happen in resources that must have some unique key.  |

## HTTP Verbs
|Verb				|Description			|
|:-------------	|:-----------------|
|GET				|Used for retrieving resources.|
|POST				|Used for creating resources.  |
|PUT				|Used for update resources.    |
|DELETE			|Used for deleting resources.  |

## Authentication
All requests require authentication. Requests that require authentication will return `403 Forbidden` if not authenticated.

Authenticating with invalid credentials will return `401 Unauthorized`.

## Pagination
Requests that return multiple items will be paginated to 20 items by default. You can specify further pages with the `?page` parameter. For some resources, you can also set a custom page size up to 100 with the `?size` parameter. 

```
curl 'https://api.adaphi.com/resources?page=2&size=100'
```

Page numbering is 1-based and that omitting the `?page` parameter will return the first page.

## Rate Limiting
For all requests, you can make up to 1250 requests per 15 minutes.
You can check the returned HTTP headers of any API request to see your current rate limit status:

```
curl -i https://api.adaphi.com
HTTP/1.1 200 OK
Status: 200 OK
X-RateLimit-Limit: 1250
X-RateLimit-Remaining: 1249
X-RateLimit-Reset: 1372700873
```

The headers tell you everything you need to know about your current rate limit status:

|Header Name				|Description		|
|:--------------------	|:---------------|
|X-RateLimit-Limit		|The maximum number of requests per 15 minutes.|
|X-RateLimit-Remaining	|The number of requests remaining in the current rate limit window.|
|X-RateLimit-Reset		|The time at which the current rate limit window resets.|

Once you go over the rate limit you will receive an error response:

```
HTTP/1.1 403 Forbidden
Status: 403 Forbidden
X-RateLimit-Limit: 1250
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1377013266

{
   "message": "API rate limit exceeded for xxx.xxx.xxx.xxx.",
   "documentation_url": "http://adaphi.readthedocs.io/en/latest/api/Overview/#rate-limiting"
}
```

## Cross Origin Resource Sharing
The API supports Cross Origin Resource Sharing (CORS) for AJAX requests from any origin.