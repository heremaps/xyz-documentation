# Search by Tags

> #### Note
>
> The endpoint for the API is <https://xyz.api.here.com/hub>.

Do you have Tags assigned to your features? Then you can search your features by them, as follows:

## Simple Search

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/searchForFeatures)*

```HTTP
GET /spaces/{spaceId}/search?tags=mountain
```

### Response

```JSON
{
  "type": "FeatureCollection",
  "features": [
    {
      "id": "{featureId}",
      "bbox": [
        -125.8948,
        52.2131,
        -125.8948,
        52.2131
      ],
      "type": "Feature",
      "properties": {
        "name": "Glacier Mountain",
        "@ns:com:here:xyz": {
          "tags": [
            "mountain",
            "canada"
          ],
          "space": "{spaceId}",
          "createdAt": 1529927311578,
          "updatedAt": 1529927778325
        }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          -125.8948,
          52.2131
        ]
      }
    }
  ]
}
```

## Advanced Search

This was the simplest version of a search by tags.  Imagine having all the restaurants of your city or your region in your space. Of course you have tagged them diligently by food category, food type and cuisine. A user of your application could be interested in eating Indian food tonight if it was vegan. He would settle for vegetarian if there were only non-vegan Indian restaurants. The search request for the HERE Data Hub could look something like that:

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/searchForFeatures)*

```HTTP
GET /spaces/{spaceId}/search?tags=vegan+indian,vegetarian
```

### Response

```JSON
{
  "type": "FeatureCollection",
  "features": [
    {
      "id": "{featureId}",
      "bbox": [ ...  ],
      "type": "Feature",
      "properties": {
        "name": "...",
        "@ns:com:here:xyz": {
          "tags": [
            "vegetarian",
            "mexican"
          ],
          "space": "{spaceId}",
          "createdAt": ...,
          "updatedAt": ...
        }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [ ... ]
      }
    },
    {
      "id": "{featureId}",
      "bbox": { ... },
      "type": "Feature",
      "properties": {
        "name": "...",
        "@ns:com:here:xyz": {
          "tags": [
            "vegan",
            "indian"
          ],
          "space": "{spaceId}",
          "createdAt": ...,
          "updatedAt": ...
        }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [ ... ]
      }
    }
  ]
}
```
