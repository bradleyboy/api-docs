# Albums

## Get all albums

```shell
curl -H "Accepts: application/json" \
     http://mysite.com/api.php?/albums
```

Without authentication, this endpoint retrieves all top-level public albums. When authenticated, unlisted albums are also returned. Unless the `flat` parameter is used, only the first level albums will be returned. To get a set's albums, see "Get album contents".

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

## Get albums tree

```shell
curl -H "Accepts: application/json" \
     http://mysite.com/api.php?/albums/tree
```

> Get unlisted tree:

```shell
curl -H "Accepts: application/json" \
     -H "X-Koken-Token: your-token-here" \
     http://mysite.com/api.php?/albums/tree/listed:0
```

Get hierarchical view of the album tree.

### Endpoint

`GET /albums/tree`

Parameter | Default | Description | Valid values
--------- | ------- | ----------- | ------------
listed | true | Whether to return listed or unlisted album tree (unlisted requires authentication). | true or false
include_empty | true | Whether to return empty albums in the result. | true or false

## Get a specific album

```shell
curl -H "Accepts: application/json" \
     http://mysite.com/api.php?/albums/1
```

Get any album by numeric ID or slug.

<aside class="notice">If you attempt to retrive an unlisted album without authentication, this endpoint will return a 403 Forbidden error.</aside>

### Endpoint

`GET /albums/1` or `GET /albums/slug:my-album`

### Query Parameters

Parameter | Default | Description | Valid values
--------- | ------- | ----------- | ------------
neighbors | 2 | How many neigboring albums to load. | Any even number.
include_empty_neighbors | false | Whether to load empty albums when looking for neighboring albums. | true or false

## Get album content

```shell
curl -H "Accepts: application/json" \
     http://mysite.com/api.php?/albums/1/content
```

Get any album and its member content (images for a standard album, albums for a set).

<aside class="notice">If you attempt to retrive an unlisted album without authentication, this endpoint will return a 403 Forbidden error.</aside>

### Endpoint

`GET /albums/1/content` or `GET /albums/slug:my-album/content`

### Query Parameters

TODO (only list params that differ from album or content listing)