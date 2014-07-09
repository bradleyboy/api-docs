---
title: Koken API Reference

language_tabs:
  - shell

includes:
  - errors

search: true
---

# Introduction

Every installation of Koken exposes an API that can be used to read and write data from that install.

To make a request to the API, you will need the install's API path, which is the domain and path to the Koken installation. For the rest of this document, we'll assume that the API path is:

`http://myphotosite.com/koken`

# Making a request

```shell
curl http://myphotosite.com/koken/api.php?/content
```

To make a request, you will need to form the URL to the endpoint using the supplied API path and the endpoint you are wishing to access. To determine the URL to call, think of this psuedo code:

`API Path + 'api.php?' + endpoint`

For example, to call the `/content` endpoint, the full URL would be:

`http://myphotosite.com/koken/api.php?/content`

# Response format

```shell
curl -H "Accept: application/json" http://myphotosite.com/koken/api.php?/content
```

Currently, all Koken API responses return data as JSON. For future proofing, we recommend explicitly passing the `Accept` header.

# Errors

> Accessing an image that does not exist

```shell
curl -i -H "Accepts: application/json" http://koken.dev:8888/api.php?/content/2222

HTTP/1.1 404 Not Found
Server: Apache/2.4.7 (Ubuntu)
Content-Length: 95
Content-Type: application/json

{
  "request": "/api.php?/content/2222",
  "error": "Content with ID: 2222 not found.",
  "http": "404"
}
```

When the Koken API encounters an error, it returns an appropriate HTTP error code as well as a JSON payload with more information about the error.

# Authentication

> To authorize, send the token in a custom HTTP header:

```shell
curl -H "Accepts: application/json" \
     -H "X-Koken-Token: your-token-here" \
     http://myphotosite.com/koken/api.php?/content
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
     http://myphotosite.com/koken/api.php?/albums/page:2/limit:10
```

Any API endpoint that returns a list will also return information that can be used to paginate over that list.

# Parameters

```shell
curl -H "Accepts: application/json" \
     http://myphotosite.com/koken/api.php?/albums/order_by:published_on/order_direction:desc
```

Because Koken's entire endpoint is contained in the query string, parameters are passed as key/value pairs in the URL itself. All parameters should be placed at the end of the API URL.

> Boolean parameters should be passed as a 0 (for false) or 1 (for true);

```shell
curl -H "Accepts: application/json" \
     http://myphotosite.com/koken/api.php?/albums/include_empty:0
```

# Albums

## Get all albums

```shell
curl -H "Accepts: application/json" \
     http://myphotosite.com/koken/api.php?/albums
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

Without authentication, this endpoint retrieves all public albums. When authenticated, unlisted albums are also returned.

### Endpoint

`GET /albums`

### Query Parameters

Parameter | Default | Description | Valid values
--------- | ------- | ----------- | ------------
day | false | Return albums from a particular day. Requires a valid year and month parameter. | Any valid day. ex: 1
day_not | false | Return albums from all buy one particular day. Requires a valid year and month parameter. | Any valid day. ex: 1
flat | false | Return all albums in the list, regardless of their hierachacal level. | true or false
id_not | false | Exclude albums from the list by id. | Any album ID. Exclude multiple IDs using a comma. Ex: 1,2
include_empty | true | If false, albums containing no content are omitted from the results. | true or false
limit | false | Limit the number of albums to return per page of results. | Any number greater than zero.
listed | true | If false, unlisted albums are retured as well. Requires authorization. | true or false
match_all_tags | false | When using tags or tags_not, whether to look for all of the supplied tags. | true or false
month | false | Return albums from a particular month. Requires a valid year parameter. | Any valid month. ex: 1
month_not | false | Return albums from all buy one particular month. Requires a valid year parameter. | Any valid month. ex: 1
order_by | title | What data is used to sort the returned list. | title, published_on, modified_on, created_on
order_direction | ASC | Whether to sort in ASC or DESC order. | ASC or DESC
page | 1 | See pagination. | Any number from 1 to total_pages.
search | false | String to search by. Search is performed on the album's title, description and tag fields unless search_filter is defined. | Any string
search_filter | false | Limit the fields used in the search. | title, description or tags
tags | false | Search for albums with specific tags. | Any string. Separate multiple tags with a comma.
tags_not | false | Search for albums without specific tags. | Any string. Separate multiple tags with a comma.
types | all | Return only sets or standard albums. | all, set or standard
with_covers | true | Whether or not to return cover data with each album | true or false
year | false | Return albums from a particular year. | Any valid year. ex: 2014
year_not | false | Return albums from all buy one particular year. | Any valid year. ex: 2014

## Get a specific album

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import 'kittn'

api = Kittn.authorize('meowmeowmeow')
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

