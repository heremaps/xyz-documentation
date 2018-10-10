# Adding and Deleting Features

!!! Note "The endpoint for the API is https://xyz.api.here.com/hub"

## Adding data

There are basically two ways of adding features to your space. And their only difference is the way, the existing data in your space is handled:
Use *POST* and any pre-existing date is retained; use *PUT* and the only data that is left in your space is the one you just uploaded with the *PUT* request.

There is a convenience request for modifying features, but that is a subject for another example.

!!! Note "Think about what tags to use before uploading and add them via the **addTags** query parameter. There is no method to add tags to all features without specifying IDs, yet."

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

```HTTP
POST /spaces/{spaceId}/features
```

with the corresponding body:

### Request body

```JSON
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        "featureclass": "River"
      },
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [
            ...
          ], ... [
              ...
          ]
        ]
      }
    }
  ]
}
```

### Response

```JSON
{
  "features": [
    {
      "geometry": {
        "type": "LineString",
       "coordinates": [
          [
            ...
          ], ... [
              ...
          ]
       ]
      },
      "id": "NTvvEciZlE",
      "type": "Feature",
      "properties": {
        "featureclass": "River",
        "@ns:com:here:xyz": {
          "createdAt": 1528461230706,
          "space": "{spaceId}",
          "tags": [
            "river"
          ],
          "updatedAt": 1528461230706
        }
      },
      "bbox": [
        44.41260826914623,
        31.5295270854107,
        45.66944420664623,
        32.563421942181535
      ]
    }
  ],
  "type": "FeatureCollection"
}
```

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

You can achieve the same with the following *PUT* request:

```HTTP
PUT /spaces/{spaceId}/features
```

with the same body:

```JSON
```

and you will get the same response:

### Response

```JSON
```

So, it is just a matter of taste and keeping the previous uploaded features.

## Delete Features

Of course, you can also delete any of the features you added previously. This is the request for it:

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

```HTTP
DELETE /spaces/{spaceId}/features/id={featureId1},{featureId2}
```

### Response

```HTTP
HTTP/1.1 204 No Content
```

If you want to delete any tagged the same, you can also do this like so:

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

```HTTP
DELETE /spaces/{spaceId}/features?tags=oldFeatures
```

The response here should be the same as it was with the delete by ID.

You can even delete all of your features by using the following request:

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

```HTTP
DELETE /spaces/{spaceId}/features?tags=*
```
