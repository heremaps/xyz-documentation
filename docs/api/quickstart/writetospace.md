# Writing to a Space

Having created the new space, you can write to it. The following example adds a collection of features.

## Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

```HTTP
POST /spaces/{spaceId}/features
```

The POST request requires a body like the following example:


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

## Response

The response is a FeatureCollection, containing all created features.

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

When you upload a feature to XYZ Hub, we automatically add a property *@ns:com:here:xyz* to it. The following information is recorded in this property:

+ *createdAt* - date and time the feature was created in milliseconds since 01.01.1970
+ *updatedAt* - date and time the feature was updated in milliseconds since 01.01.1970
+ *space* - random unique space ID, created at space creation, a string
+ *tags* - the tags you added to the space, an array of strings

We use the namespace to store the additional information in the object without interfering with the properties you provided.