# Access Features

This section describes how to get features from HERE Data Hub by using a bounding box or a tile, and iterating features.

## Get Features by Bounding Box

You can get features from Spaces using a bounding box.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeaturesByBBox)*

```HTTP
GET /spaces/{spaceId}/bbox?west={westLongitude}&north={northLatitude}&east={eastLongitude}&south={southLatitude}
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
                "@ns:com:here:xyz":
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

## Get Features in a Tile

You can get features from Spaces using a tileID.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeaturesByTile)*

```HTTP
GET /spaces/{spaceId}/tile/{type}/{tileId}
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
                "@ns:com:here:xyz":
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

## Get Features for Iteration

The request allows iterating through the features in Spaces. The features in the response are ordered so that no feature is returned twice within one iteration. However, the features are modified concurrently so all features may not be contained within an iteration.

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
                "@ns:com:here:xyz":
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
