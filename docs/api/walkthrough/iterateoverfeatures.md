# Iterate over Features

> #### Note
>
> The endpoint for the API is <https://xyz.api.here.com/hub>.

Iterating over features is different from search in two ways:

1. The results are ordered, no features are returned twice.
2. Searches can be continued over several requests.

Sometimes your search will have a lot of features as a result. But the limit you explicitly set or the default limit will only return some of them. In this case, a root attribute **handle** is set in the response. The search can then be continued with the next feature in line by adding the value of the **handle** response attribute as a **handle** query parameter to the request.

## Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Features)*

```HTTP
GET /spaces/{spaceId}/iterate?tags=&limit=2&handle=2
```

## Response

```JSON
{
  "handle": "4",
  "type": "FeatureCollection",
  "features": [
    {
      "id": "{featureId}",
      "bbox": [
        -123.291,
        50.1203,
        -123.291,
        50.1203
      ],
      "type": "Feature",
      "properties": {
        "name": "Mount Cayley",
        "@ns:com:here:xyz": {
          "tags": [
            "canada",
            "mountain"
          ],
          "space": "{spaceId}",
          "createdAt": 1529855978027,
          "updatedAt": 1529855978027
        }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          -123.291,
          50.1203
        ]
      }
    },
    {
      "id": "{featureId}",
      "bbox": [
        -124.162,
        48.9511,
        -124.162,
        48.9511
      ],
      "type": "Feature",
      "properties": {
        "name": "Mount Whymper",
        "@ns:com:here:xyz": {
          "tags": [
            "canada",
            "mountain"
          ],
          "space": "{spaceId}",
          "createdAt": 1529855978028,
          "updatedAt": 1529855978028
        }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          -124.162,
          48.9511
        ]
      }
    }
  ]
}
```
