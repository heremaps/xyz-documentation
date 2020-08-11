# Search for Features

This section describes how to search for specific features in your space on the basis of the
properties. This is a powerful tool to retrieve just a specific subset of the content in
your space.

## Check which properties are searchable

Not all Feature properties are necessarily searchable. So before using the property search it
makes sense to check which of the properties in your space can be searched.

> #### Note
>
> Data Hub has a space-specific algorithm to automatically decide which of the space's properties
> are searchable. In case you desire other properties to be searchable please have a look into
> the guide at ["Adjust searchable properties"](searchableproperties.md).*

To check which of the properties in your space a search can be performed on, please have a look
into the space's statistics.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getStatistics)*

```HTTP
GET /spaces/{spaceId}/statistics
```

### Response

```JSON
{
    "type": "StatisticsResponse",
    "count": {
        "value": 29208,
        "estimated": true
    },
    "byteSize": {
        "value": 108364,
        "estimated": true
    },
    "bbox": {
        "value": [
            -10,
            -10,
            10,
            10
        ],
        "estimated": true
    },
    "geometryTypes": {
        "value": [
            "Point"
        ],
        "estimated": true
    },
    "properties": {
        "value": [
            {
                "key": "Route",
                "count": 29208,
                "searchable": false
            },
            {
                "key": "Route Type",
                "count": 29208,
                "searchable": true
            }
        ],
        "estimated": true,
        "searchable": "PARTIAL"
    },
    "tags": {
        "value": [
            {
                "key": "PuneBusStop",
                "count": 29208
            }
        ]
    }
}
```

The StatisticsResponse above, shows the searchable property having the value PARTIAL.
This is a global indicator which can have one of the following values:

- `NONE` *(No properties in your space are searchable, so no search queries can be performed)*
- `PARTIAL` *(Some of the properties are searchable)*
- `ALL` *(All of the properties are searchable)*

In case of `PARTIAL` you can find the more detailed `searchable` boolean flags inside the
`value`-array of the `properties`-map.

## Search for features in the space

Using one of the API endpoints
[`/spaces/{spaceId}/search`](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/searchForFeatures),
[`/spaces/{spacesId}/bbox`](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeaturesByBBox)
or
[`/spaces/{spaceId}/tile`](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeaturesByTile),
you can retrieve a set of features in your space matching a specified
query. Such a query could look like:

```HTTP
GET /spaces/{spaceId}/search?p.someProperty1=value1,value2
```

Here the prefix `p.` points to the feature's properties map.
The resulting FeatureCollection will contain all features having `value1` **or** `value2` for the
property `property_name_1`.

Filtering on values of the properties `someProperty1` **and** `someProperty2` a query could look
like:

```HTTP
GET /spaces/{spaceId}/search?p.someProperty1=value1,value2&p.someProperty2<527
```

The available operators are:

- "=" - equals
- "!=" - not equals
- ">=" or "=gte=" - greater than or equals
- "<=" or "=lte=" - less than or equals
- ">" or "=gt=" - greater than
- "<" or "=lt=" - less than

The response will contain only the features matching all conditions in the query.
In case of the bounding-box or tile queries, the search is only applied to the features located in
the specified area.
