This page describes the resources that make up the Adaphi API v1.

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

## HTTP Verbs

## Authentication

## Hypermedia

## Pagination

## Rate Limiting

## User Agent Required

## Conditional requests

## Cross Origin Resource Sharing

## Timezones