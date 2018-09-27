# Working with Features

## Reading Features in a Space by ID

The following request queries a single feature using the ID of the feature.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Features)*

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

## Reading Features of a Space

For querying multiple features in a space use the following request.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Features)*

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

## Getting a Feature Count

Here is an example of getting the number of features in a space.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Features)*

```HTTP
GET /spaces/{spaceId}/count
```

### Response

```JSON
{
  "type": "FeatureCollection",
  "features": [],
  "count": 42
}
```

## Creating/Replacing Features

To create features in a space or to replace existing ones you can use the following request.

!!! Warning "Note: Existing features will be completely erased by using this PUT request"

To keep already existing features use [this request](#modifying-space-features).

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

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

## Modifying Features

This is an example for modifying existing features using a POST request.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

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

## Partially Updating Features in Space

This request contains only the feature properties you want to add, update or delete.

- If the property does not yet exist in the latest version of the feature it is added.
- If the property has another value of the latest version of the feature it is updated to the value in the request
- If the property value is null in the request, the property is deleted from the feature object

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

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

## Searching a Space for Features

There are two ways of searching a space. /search is one of them, the other is [/iterate](#iterating-features-from-specific-spaces). This does not order the results and it does not enable you to continue the search.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Features)*

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

## Iterating Features from Specific Spaces

This is the second way to search a space (the other, you guessed it, is [/search](#searching-a-space-for-features)). Iterate allows you to iterate over all the matching features with the handle *handle* as a query parameter

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Features)*

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

## Deleting Multiple Features

You can get rid of specific features by sending this request with their feature IDs

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Features)*

```HTTP
DELETE /spaces/{spaceId}/features?id={id1},{id2}
```

### Response

```HTTP
HTTP/1.1 204 No Content
```