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

and you will get the same response:

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

So, it is just a matter of taste and keeping the previous uploaded features.

### Validation Errors

If you are using the validation feature, you sometimes will get an error message when adding or modifying features, such as this one:

```JSON
{
  "type": "FeatureCollection",
  ...
  "features": [],
  "failed": [
    {
      "id": null,
      "position": 0,
      "message": "Feature on position 0 has JSON schema validations errors/warnings.\n[[1,151][/properties] The object must have a property whose name is \"city\"., [1,151][/properties] The object must have a property whose name is \"employees\"., [1,151][/properties] The object must have a property whose name is \"name\"., [1,151][/properties] The object must have a property whose name is \"country\".]"
    }
  ]
}
```

The *failed* property contains all the features that schema validation rejected.  
The *id* is the id you sent. If you did not send one, the id will be null as in the example.  
*Position* is the position (zero-based) in the uploaded feature collection.  
The *message* contains the schema validation errors with a detailed description of what does not confirm to your schema.

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
