# Using Feature Clustering

This section describes how to retrieve features from XYZ Hub inside a tile or bounding-box in a
clustered form.

## Get clustered Features in a Bounding Box

While retrieving features from Spaces inside a tile or bounding box you can use the "hexbin"
clustering to visualize your data as hexagons. Each hexagon represents the features of the area it
covers. Additional statistical information about one property of your data can be evaluated and
returned as properties of the returned hexagonal features.

The hexbin algorithm divides the world in hexagonal "bins" on a specified resolution.
Each hexagon has an address being described by the H3 addressing scheme.
For more information on that topic see: https://eng.uber.com/h3/

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeaturesByTile)*

```HTTP
GET /spaces/{spaceId}/tile/{type}/{tileId}?clustering=hexbin&clustering.resolution={aNumber}&clustering.property={aPropertyName}
```

### Response

```JSON
{
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "id": "881f1d4f4dfffff",
            "properties": {
                "kind": "H3",
                "propertyStatistics": {
                    "qty": 10,
                    "min": 1.0,
                    "max": 7.8,
                    "avg": 4.2,
                    "sum": 33.1
                },
                "centroid": [
                    13.4100054,
                    52.5800028
                ]
            },
            "geometry": {
                "type": "Polygon",
                "coordinates": [
                    [
                        [
                            13.404696,
                            52.5829628
                        ],
                        [
                            13.4024784,
                            52.5787237
                        ],
                        [
                            13.4077874,
                            52.5757636
                        ],
                        [
                            13.4153143,
                            52.5770425
                        ],
                        [
                            13.4175328,
                            52.5812816
                        ],
                        [
                            13.4122236,
                            52.5842418
                        ],
                        [
                            13.404696,
                            52.5829628
                        ]
                    ]
                ]
            }
        },
        {
            "type": "Feature",
            "id": "881f1d4a81fffff",
            "properties": {
                "kind": "H3",
                "propertyStatistics": {
                    "qty": 2,
                    "min": 3,
                    "max": 6,
                    "avg": 4.5,
                    "sum": 9
                },
                "centroid": [
                    13.3520171,
                    52.5739963
                ]
            },
            "geometry": {
                "type": "Polygon",
                "coordinates": [
                    [
                        [
                            13.3467067,
                            52.5769538
                        ],
                        [
                            13.3444934,
                            52.5727137
                        ],
                        [
                            13.3498035,
                            52.5697562
                        ],
                        [
                            13.357327,
                            52.5710387
                        ],
                        [
                            13.3595411,
                            52.5752788
                        ],
                        [
                            13.3542309,
                            52.5782365
                        ],
                        [
                            13.3467067,
                            52.5769538
                        ]
                    ]
                ]
            }
        }
    ]
}
```