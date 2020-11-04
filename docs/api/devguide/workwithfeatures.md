# Work with Features

## Read Features in a Space by ID

The following request queries a single feature using the ID of the feature.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeature)*

```HTTP
GET /spaces/{spaceId}/features/{featureId}
```

### Response

```JSON
{
  "type": "Feature",
  "id": "{featureId}",
  "geometry":
  {
    "type": "Point",
    "coordinates":
    [
      -2.960847,
      53.430828
    ]
  },
  "properties":
  {
    "name": "Anfield",
    "spaceId@ns:com:here:xyz":
        {},
        "amenity": "Football Stadium",
        "capacity": 54074,
        "popupContent": "Home of Liverpool Football Club"
  }
}
```

## Read Features of a Space

For querying multiple features in a space use the following request:

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeatures)*

```HTTP
GET /spaces/[spaceId}/features?id={featureId1},{featureId2},{featureId3}
```

### Response

```JSON
{
    "type": "FeatureCollection",
    "features":
    [
      {
        "type": "Feature",
        "geometry":
        {
          "type": "Point",
          "coordinates":
          [
            -2.960847,
            53.430828
          ]
        },
        "properties":
        {
           "spaceId@ns:com:here:xyz":
           {
             "tags":
             [
               "football",
               "stadium"
             ]
            },
            "name": "Anfield",
            "amenity": "Football Stadium",
            "capacity": 54074,
            "popupContent": "Home of Liverpool Football Club"
          }
      }
    ]
}
```

## Get a Feature Count and other statistics

Here is an example of getting the number of features in a space, the size and a list of tags on the space.

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
        "searchable": true
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

## Create/Replace Features

To create features in a space or to replace existing ones you can use the following request.

> #### Warning
>
>Existing features will be completely erased by using this PUT request.

To keep already existing features, use [this request](#modifying-space-features).

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Features/putFeatures)*

```HTTP
PUT /spaces/{spaceId}/features
```

with the following body

```JSON
{
    "type": "FeatureCollection",
    "features":
    [
      {
        "type": "Feature",
        "geometry":
        {
          "type": "Point",
          "coordinates":
          [
            -2.960847,
            53.430828
          ]
        },
        "properties":
        {
          "spaceId@ns:com:here:xyz":
          {
            "tags":
            [
              "football",
              "stadium"
            ]
          },
          "name": "Anfield",
          "amenity": "Football Stadium",
          "capacity": 54074,
          "popupContent": "Home of Liverpool Football Club"
        }
      }
  ]
}
```

### Response

```JSON
{
    "type": "FeatureCollection",
    "features":
    [
      {
        "type": "Feature",
        "id": "BfiimUxHjj",
        "geometry":
        {
          "type": "Point",
          "coordinates":
          [
            -2.960847,
            53.430828
          ]
        },
        "properties":
        {
          "name": "Anfield",
          "spaceId@ns:com:here:xyz":
          {
             "createdAt": 1517504700726,
             "updatedAt": 1517504700726,
             "space": "x-demospace",
             "tags":
             [
               "football",
               "stadium"
             ]
           },
           "amenity": "Football Stadium",
           "capacity": 54074,
           "popupContent": "Home of Liverpool Football Club"
         }
      }
   ]
}
```

## Modify Features

This is an example for modifying existing features using a POST request.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Features/postFeatures)*

```HTTP
POST /spaces/{spaceId}/features
```

with the following body

```JSON
{
    "type": "FeatureCollection",
    "features":
    [
      {
        "type": "Feature",
        "geometry":
          {
            "type": "Point",
            "coordinates":
            [
              -2.960847,
              53.430828
            ]
          },
          "properties":
          {
            "spaceId@ns:com:here:xyz":
            {
              "tags":
              [
                "football",
                "stadium"
              ]
            },
            "name": "Anfield",
            "amenity": "Football Stadium",
            "capacity": 54074,
            "popupContent": "Home of Liverpool Football Club"
          }
        }
    ]
}
```

### Response

```JSON
{
    "type": "FeatureCollection",
    "features":
    [
      {
        "type": "Feature",
        "id": "BfiimUxHjj",
        "geometry":
        {
          "type": "Point",
          "coordinates":
          [
            -2.960847,
            53.430828
          ]
        },
        "properties":
        {
          "name": "Anfield",
          "spaceId@ns:com:here:xyz":
          {
            "createdAt": 1517504700726,
            "updatedAt": 1517504700726,
            "space": "x-demospace",
            "tags":
            [
              "football",
              "stadium"
            ]
          },
          "amenity": "Football Stadium",
          "capacity": 54074,
          "popupContent": "Home of Liverpool Football Club"
        }
      }
  ]
}
```

## Partially Update Features in Space

This request contains only the feature properties you want to add, update or delete.

- If the property does not yet exist in the latest version of the feature it is added.
- If the property has another value of the latest version of the feature it is updated to the value in the request
- If the property value is null in the request, the property is deleted from the feature object

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Features/patchFeature)*

```HTTP
PATCH /spaces/{spaceId}/features?id={featureId1},{featureId2},{featureId3}
```

A PATCH request needs something like the following body

```JSON
{
    "type": "Feature",
    "id": "string",
    "geometry":
    {
      "type": "string"
    },
    "properties":
    {
       "spaceId@ns:com:here:xyz":
       {}
    },
    "bbox":
    [
      -100.1,
      -1.1,
      100.1,
      1.1
    ]
}
```

### Response

```JSON
{
  "type": "FeatureCollection",
  "features":
  [
    {
      "type": "Feature",
      "id": "BfiimUxHjj",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -2.960847,
          53.430828
        ]
      },
      "properties": {
        "name": "Anfield",
        "spaceId@ns:com:here:xyz": {
          "createdAt": 1517504700726,
          "updatedAt": 1517504700726,
          "space": "x-demospace",
          "tags": [
            "football",
            "stadium"
          ]
        },
        "amenity": "Football Stadium",
        "capacity": 54074,
        "popupContent": "Home of Liverpool Football Club"
      }
    }
  ]
}
```

## Validation Errors

If you are using the validation feature, you sometimes will get an error message when uploading or modifying features, such as the following:

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

## Search a Space for Features

There are two ways of searching a space. /search is one, the other is [/iterate](#iterating-features-from-specific-spaces). This does not order the results and it does not enable you to continue the search.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/searchForFeatures)*

```HTTP
GET /spaces/{spaceId}/search
```

### Response

```JSON
{
    "type": "FeatureCollection",
    "features":
    [
      {
        "type": "Feature",
        "id": "BfiimUxHjj",
        "geometry":
        {
          "type": "Point",
          "coordinates":
          [
            -2.960847,
            53.430828
          ]
        },
        "properties":
        {
          "name": "Anfield",
          "spaceId@ns:com:here:xyz":
          {
            "createdAt": 1517504700726,
            "updatedAt": 1517504700726,
            "space": "x-demospace",
            "tags":
            [
              "football",
              "stadium"
            ]
          },
          "amenity": "Football Stadium",
          "capacity": 54074,
          "popupContent": "Home of Liverpool Football Club"
        }
      }
    ]
}
```

## Iterate Features from Specific Spaces

This is the second way to search a space (the other is [/search](#searching-a-space-for-features)). Iterate allows you to iterate over all the matching features with the handle *handle* as a query parameter

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/iterateFeatures)*

```HTTP
GET /spaces/{spaceId}/iterate
```

### Response

```JSON
{
    "type": "FeatureCollection",
    "features":
    [
      {
        "type": "Feature",
        "id": "BfiimUxHjj",
        "geometry":
        {
          "type": "Point",
          "coordinates":
          [
            -2.960847,
            53.430828
          ]
        },
        "properties":
        {
          "name": "Anfield",
          "spaceId@ns:com:here:xyz":
          {
            "createdAt": 1517504700726,
            "updatedAt": 1517504700726,
            "space": "x-demospace",
            "tags":
            [
              "football",
              "stadium"
            ]
           },
           "amenity": "Football Stadium",
           "capacity": 54074,
           "popupContent": "Home of Liverpool Football Club"
          }
      }
  ]
}
```

## Delete Multiple Features

You can remove specific features by sending this request with their feature IDs

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Features/deleteFeatures)*

```HTTP
DELETE /spaces/{spaceId}/features?id={id1},{id2}
```

### Response

```HTTP
HTTP/1.1 204 No Content
```
