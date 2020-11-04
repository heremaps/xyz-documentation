# Namespaces

Namespaces are used to group objects unambiguously, so they can be addressed without confusion. They are used in several areas like XML and Object-oriented programming.

## Example

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

```HTTP
PUT /spaces/{spaceId}/features?addTags=mountain,canada
```

with the following body

```JSON
{
    "type":"FeatureCollection",
    "features":
    [
      {
        "type":"Feature",
        "properties":
        {
          "name":"Baldy Mountain"
        },
        "geometry":
        {
          "type":"Point",
          "coordinates":[-100.728, 51.4686]
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
        "type": "Point",
        "coordinates": [
          -100.728,
          51.4686
        ]
      },
      "id": "HsOsZ0FXUI",
      "type": "Feature",
      "properties": {
        "name": "Baldy Mountain",
        "@ns:com:here:xyz": {
          "createdAt": 1529855978032,
          "space": "{spaceId}",
          "tags": [
            "canada",
            "mountain"
          ],
          "updatedAt": 1529855978032
        }
      },
      "bbox": [
        -100.728,
        51.4686,
        -100.728,
        51.4686
      ]
    }
  ],
  "type": "FeatureCollection"
}
```

When you upload a feature to Data Hub, HERE automatically adds a property *@ns:com:here:xyz* to it. The following information is recorded in this property:

+ *createdAt* - date and time the feature was created in milliseconds since 01.01.1970
+ *updatedAt* - date and time the feature was updated in milliseconds since 01.01.1970
+ *space* - random unique space ID, created at space creation, a string
+ *tags* - the tags you added to the space, an array of strings

We use the namespace to store additional information in the object without interfering with the properties you provided.
