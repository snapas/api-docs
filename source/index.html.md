---
title: Snap.as API Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - go

toc_footers:
  - <a href='https://developers.snap.as/'>Snap.as Developers</a>
  - <a href='https://twitter.com/snap_as'>@snap_as</a>
  - <a href='https://we.snap.as/'>Updates</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation by Slate</a>

includes:

search: true

code_clipboard: true
---

# Introduction

Welcome to the [Snap.as](https://snap.as) API! Our API lets you build your own image-based applications or utilities on our platform.

Snap.as is part of the [Write.as](https://write.as) suite of apps, and follows a similar API design. So if you're familiar with the [Write.as / WriteFreely API](https://developers.write.as/docs/api/), working with the Snap.as API should be fairly straightforward.

<aside class="notice">
This API documentation is only valid for Snap.as users created after October 6, 2020. It will apply to all users in the near future.
</aside>

## Base API

Call all requests on this base URL: `https://v2.snap.as`

## Errors

> Failed requests return with an error `code` and `error_msg`, like below.

```json
{
  "code": "400",
  "error_msg": "Bad request."
}
```

The Snap.as API uses conventional HTTP response codes to indicate the status of an API request. Codes in the `2xx` range indicate success or that more information is available in the returned data, in the case of bulk actions.
Codes in the `4xx` range indicate that the request failed with the information provided. Codes in the `5xx` range indicate an error with the Snap.as servers (you shouldn't see many of these).

Error Code | Meaning
---------- | -------
400 | Bad Request -- The request didn't provide the correct parameters, or JSON / form data was improperly formatted.
401 | Unauthorized -- No valid user token was given.
403 | Forbidden -- You attempted an action that you're not allowed to perform.
404 | Not Found -- The requested resource doesn't exist.
405 | Method Not Allowed -- The attempted method isn't supported.
410 | Gone -- The entity was unpublished, but may be back.
429 | Too Many Requests -- You're making too many requests, especially to the same resource.
500, 502, 503 | Server errors -- Something went wrong on our end.

## Responses

> Successful requests return with a `code` in the `2xx` range and a `data` object or array, depending on the request.

```json
{
  "code": "200",
  "data": {
  }
}
```

All responses are returned in a JSON object containing two fields:

Field | Type | Description
----- | ---- | -----------
`code` | integer | An HTTP status code in the `2xx` range.
`data` | object or array | A single object for most requests; an array for bulk requests.

This wrapper will never contain an `error_msg` property at the top level.

# Authentication

All endpoints require authentication. You'll need to pass a Write.as user access token with any requests:

`Authorization: 00000000-0000-0000-0000-000000000000`

See the [Authenticate a User](https://developers.write.as/docs/api/#authenticate-a-user) section for information on logging in.

<aside class="notice">
Replace <code>00000000-0000-0000-0000-000000000000</code> with a user's access token.
</aside>

# Photos

## Upload a Photo

```shell
curl "https://v2.snap.as/api/photos/upload" \
  -H "Authorization: 00000000-0000-0000-0000-000000000000" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@photo.jpeg"
```

> Example Response

```json
{
  "code": 201,
  "data": {
    "id": "aBCd5678",
    "body": null,
    "filename": "photo.jpeg",
    "size": 219995,
    "url": "https://i.snap.as/aBCd5678.jpeg",
  }
}
```

This uploads a new photo to the authenticated user's account.

### Definition

`POST /api/photos/upload`

### Arguments

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
**file** | file | yes | The photo to be uploaded.

### Returns

A `201 Created` HTTP status and the newly created photo object.

## Delete a Photo

```shell
curl "https://v2.snap.as/api/photos/aBCd5678" \
  -X DELETE \
  -H "Authorization: 00000000-0000-0000-0000-000000000000"
```

This deletes a photo from the authenticated user's account.

### Definition

`POST /api/photos/{PHOTO_ID}`

### Arguments

None.

### Returns

A `200 OK` HTTP status.
