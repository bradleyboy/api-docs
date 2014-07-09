---
title: Koken API Reference

language_tabs:
  - shell

includes:
  - albums

search: true
---

# Introduction

Every installation of Koken exposes an API that can be used to read and write data from that install.

To make a request to the API, you will need the install's API path, which is the domain and path to the Koken installation. The user of the install will need to supply your application with the appropriate token by logging in to their Koken install and visiting Settings > Applications.

For the rest of this document, we'll assume that the API path is:

`http://mysite.com/`

# Making a request

```shell
curl http://mysite.com/api.php?/content
```

To make a request, you will need to form the URL to the endpoint using the supplied API path and the endpoint you are wishing to access. To determine the URL to call, think of this psuedo code:

`API Path + 'api.php?' + endpoint`

For example, to call the `/content` endpoint, the full URL would be:

`http://mysite.com/api.php?/content`

# Response format

```shell
curl -H "Accept: application/json" \
     http://mysite.com/api.php?/content
```

Currently, all Koken API responses return data as JSON. In the future, Koken may provide other response formats, so we recommend explicitly passing the `Accept` HTTP header.

# Errors

> Accessing an image that does not exist:

```shell
curl -i -H "Accepts: application/json" \
     http://koken.dev:8888/api.php?/content/2222

HTTP/1.1 404 Not Found

{
  "request": "/api.php?/content/2222",
  "error": "Content with ID: 2222 not found.",
  "http": "404"
}
```

When the Koken API encounters an error, it returns an appropriate HTTP error code as well as a JSON payload with more information about the error.

# Authentication

> To authorize, send the token in the X-Koken-Token custom HTTP header:

```shell
curl -H "Accepts: application/json" \
     -H "X-Koken-Token: your-token-here" \
     http://mysite.com/api.php?/content
```

To read public data from a Koken install, authorization is not required. To read unlisted or private information, or to make modifications to content via the API, authorization is required via *tokens*. There are two levels of token permissions:

Permissions | Description
--------- | -----------
read | Access to all public, unlisted and private content, albums and essays.
read-write | Same permissions as read but also with permission to create and delete.

The user of the install will need to generate and supply your application with the appropriate token by logging in to their Koken install and visiting `Settings > Applications`.

# Pagination

> Example JSON info included in list responses:

```json
{
  "page": 1,
  "pages": 4,
  "per_page": 10,
  "albums": [

  ]
}
```
> Get the next page

```shell
curl -H "Accepts: application/json" \
     http://mysite.com/api.php?/albums/page:2/limit:10
```

Any API endpoint that returns a list will also return information that can be used to paginate over that list.

# Parameters

```shell
curl -H "Accepts: application/json" \
     http://mysite.com/api.php?/albums/order_by:published_on/order_direction:desc
```

Because Koken's entire endpoint is contained in the query string, parameters are passed as key/value pairs in the URL itself. All parameters should be placed at the end of the API URL.

> Boolean parameters should be passed as a 0 (for false) or 1 (for true);

```shell
curl -H "Accepts: application/json" \
     http://mysite.com/api.php?/albums/include_empty:0
```