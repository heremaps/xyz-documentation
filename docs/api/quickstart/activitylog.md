# Use Activity-Log

1. Create a new space with the activity-log listener:

    ```HTTP
     POST /spaces
   ```

   *Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Spaces/postSpace)*

   ```JSON
     {
        "title": "Test space to track changes",
         "listeners": [
           {"id": "activity-log"}
         ]
     }
   ```

1. Check spaces. You will find the just created one, as well as a new one, that has a title like: Activity log for space \<newSpaceId\>

     ```HTTP
    GET /spaces
    ```

    *Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Spaces/getSpaces)*

1. Post something into your newly created space:

    ```HTTP
    PUT /spaces/<newSpaceId>/features
    ```

    *Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features/putFeatures)*

    ```JSON
    {
      "type":"FeatureCollection",
      "features":[
        {
          "id":"newFeatureId",
          "type":"Feature",
          "geometry":{
            "type":"Point",
            "coordinates":[1,0]
          }
        }
      ]
    }
    ```

    Check into your activity log space:

    ```HTTP
    GET /spaces/<activityLogSpaceId>/iterate
    ```

    *Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/iterateFeatures)*

    This results in something like this

    ```JSON
    {
      "type": "FeatureCollection",
        ...
      "features": [
        {
          "id": "<uuidOfFeature>",
          "type": "Feature",
          "properties": {
            "@ns:com:here:xyz": {
              "tags": [],
              "space": "<activityLogSpaceId>",
              "action": "SAVE",
              "original": {
                "id": "newFeatureId",
                "space": "<newSpaceId>",
                "createdAt": ...,
                "invalidatedAt": ...,
                "updatedAt": ...
              },
              "createdAt": ...,
              "updatedAt": ...
            }
          },
          "geometry": {
            "type": "Point",
            "coordinates": [
              1,
              0
            ]
          }
        }
      ]
    }
    ```

## Search for specific feature

You can search the Activity Log for a specific feature using its original id.
This request returns an unsorted list of all revisions of the object.

```HTTP
GET /spaces/<activityLogSpaceId>/search?p.@ns:com:here:xyz.original.id="newFeatureId"
```

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/searchForFeatures)*

Response

```JSON
{
  "type": "FeatureCollection",
  "features": [
    {
      "id": "<uuidOfFeature>",
      "type": "Feature",
      "properties": {
        "@ns:com:here:xyz": {
          "tags": [],
          "space": "<activityLogSpaceId>",
          "action": "SAVE",
          "original": {
            "id": "newFeatureId",
            "space": "<newSpaceId>",
            "createdAt": ...,
            "invalidAt": ...,
            "updatedAt": ...
          },
          "createdAt": ...,
          "updatedAt": ...
        }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          1,
          0
        ]
      }
    }
  ]
}
```

## Look up a specific revision of a feature

You can search for a specific revision of a feature using the *uuid* of the feature:

```HTTP
GET /spaces/<activityLogSpaceId>/features/<uuidOfFeature>
```

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeature)*

Response:

```JSON
{
  "id": "<uuidOfFeature>",
  "type": "Feature",
  "properties": {
    "@ns:com:here:xyz": {
      "tags": [],
      "space": "<activityLogSpaceId>",
      "action": "SAVE",
      "original": {
        "id": "newFeatureId",
        "space": "<newSpaceId>",
        "createdAt": ...,
        "invalidatedAt": ...,
        "updatedAt": ...
      },
      "createdAt": ...,
      "updatedAt": ...
    }
  },
  "geometry": {
    "type": "Point",
    "coordinates": [
      1,
      0
    ]
  }
}
```

## Activity Log for a certain point in time

You can search for a specific point in time by looking at the *createdAt* and *invalidatedAt* timestamps:

```HTTP
GET /spaces/<activityLogSpaceId>/search?p.@ns:com:here:xyz.original.updatedAt<=1569538800000&p.@ns:com:here:xyz.original.invalidAt=gt=1569538800000
```

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/searchForFeatures)*

Response body:

```JSON
{
  "type": "FeatureCollection",
  "features": [
    {
      "id": "<uuidOfFeature>",
      "type": "Feature",
      "properties": {
        "@ns:com:here:xyz": {
          "tags": [],
          "space": "<activityLogSpaceId>",
          "action": "SAVE",
          "original": {
            "id": "newFeatureId",
            "space": "<newSpaceId>",
            "createdAt": 1569405309000,
            "invalidAt": 9223372036854776000,
            "updatedAt": 1569405309000
          },
          "createdAt": 1569405309624,
          "updatedAt": 1569405309624
        }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          1,
          0
        ]
      }
    }
  ]
}
```
