# Spatial Search for Features

This section describes how to search for specific features in your space which are intersecting with a provided geometry.

## Possibilities to Provide a Geometry for a Spatial Search

A spatial search requires a Geometry as an input. One common use-case: find all Features which are
around 1000 meters from a given Position. Another one could be: find all Features which are 100 meters
beside a road.

You can realize that kind of spatial search by providing a Point or LineSting Geometry and a radius, indicated
in meters, which gets applied to thicken the input Geometry.

Another common usecase is: find all Features which are belonging to a country. For this, you only need to provide the Polygon Geometry of the country.  

All [GeoJSON](../concepts/geojsonbasics.md) Geometry
types are allowed as input [Point, MultiPoint, LineString, MultiLineString, Polygon, MultiPolygon].

### Submitting Geometry via POST Request

The easiest way for providing a search Geometry is submitting it via a POST-Request.  

### Referencing an existing Geometry for a Spatial Search

Another way to provide a search Geometry is to read it from a existing Feature, which is stored in a Data Hub Space
that you have access to. This is recommended if your search Geometry is very complex, or if you want to store
frequently used Geometries for spatial searches.

### Extend Spatial Search

You can combine the spatial search requests with defining:

+ *radius* - in meter to thicken the input Geometry
+ *[property search](../devguide/propertiessearch.md)* - add filter based on properties
+ *tag search* - add filter based on tags
+ *selection* - select properties which should exclusively get included in the response

## Using Spatial GET-Requests

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeaturesBySpatial)*

Spatial-Search by referencing a position (lat,lon) & defining a radius indicated in meters:

```HTTP
GET /spaces/{spaceId}/spatial?lon={Longitude}&lat={Latitude}&radius={raduisInMeters}
```

Spatial-Search by referencing a Feature from a Data Hub Space:

```HTTP
GET /spaces/{spaceId}/spatial?refSpaceId={referencedSpace}&refFeatureId
={referencedFeatureId}
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
                    7.01,
                    50.03
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

## Using Spatial POST-Request

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeaturesBySpatialPost)*

Spatial-Search by posting a Geometry:

```HTTP
POST /spaces/{spaceId}/spatial
```

with the following body

```JSON
{
    "type": "MultiPolygon",
    "coordinates": [
        [
            [
                [
                    7,
                    50
                ],
                [
                    7.1,
                    50
                ],
                [
                    7.1,
                    50.1
                ],
                [
                    7,
                    50.1
                ],
                [
                    7,
                    50
                ]
            ],
            [
                [
                    7.05,
                    50.05
                ]
                [
                    7.05,
                    50.09
                ],
                [
                    7.09,
                    50.09
                ],
                [
                    7.09,
                    50.05
                ],
                [
                    7.05,
                    50.05
                ]
            ]
        ]
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
                    7.01,
                    50.03
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
                    "tags": []
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
