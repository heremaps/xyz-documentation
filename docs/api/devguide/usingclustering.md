# Use Clustering

This section describes how to retrieve features from Data Hub inside a tile or bounding-box in a
clustered form.

## Get clustered Features in a Bounding Box

While retrieving features from Spaces inside a tile or bounding box you can use the "hexbin"
clustering to visualize your data as hexagons. Each hexagon represents the features of the area it
covers. Additional statistical information about one property of your data can be evaluated and
returned as properties of the returned hexagonal features.

The hexbin algorithm divides the world in hexagonal "bins" on a specified resolution.
Each hexagon has an address being described by the H3 addressing scheme.
For more information on that topic see: <https://eng.uber.com/h3/>

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeaturesByTile)*

```HTTP
GET /spaces/{spaceId}/tile/{type}/{tileId}?clustering=hexbin&clustering.resolution={aNumber}&clustering.property={aPropertyName}
```

The following clustering related parameters can be passed and combined with the others ( e.g. tags,clip, feature filtering).

|request parameter | value | | |
|---|---|---|---|
|clustering| string "hexbin" | | activates clustering |
|clustering.resolution| integer [0..15] | optional | The H3 hexagon resolution |
|clustering.property| string "{aPropertyName}" | optional | A property of the original features for which to calculate statistics |
|clustering.pointmode | boolean [true\|false] | optional | returns the centroid of the hexagons as geojson-features geometry  |

### Response

```JSON
{
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "properties": {
                "kind" : "H3",
                "kind_detail" : "858b1303fffffff",
                "resolution": 5,
                "level": 7,
                "aggregation": {
                    "aPropertyName": {  // only if a clustering.property={aPropertyName} is specified. If not specified field "qty" is
                                        // written on this object-level (e.g. properties.aggregation.qty )
                        "avg": 30.05000,
                        "max": 44.1,
                        "min": 16,
                        "qty": 2,
                        "sum": 60.1
                    }
                },
            "centroid": [ ... ], // only if clustering.pointmode = false|null
            "hexagon": [ ... ]  // only if clustering.pointmode = true
            },
            "geometry": {...},
        },
        ......
        ......
        {
            "type": "Feature",
            "properties": {
                "kind" : "H3",
                 "kind_detail" : "881f1d4a81fffff",
                    ....
            },
            "geometry": {...}
        }
    ]
}
```

### Miscellaneous

#### Maximum Resolution for zoomlevel

The parameter clusterning.resolution specifies the size of the hexagons wanted.
( s. <https://uber.github.io/h3/#/documentation/core-library/resolution-table> )
There is a maximum resolution per zoomlevel requested (s. table below). If the value of clusterning.resolution exeeds, then the "Max H3 Resolution" will be used.

|Zoomlevel|Max H3 Resolution|
|---|---|
|0|2|
|1|2|
|2|2|
|3|2|
|4|3|
|5|4|
|6|4|
|7|5|
|8|6|
|9|6|
|10|7|
|11|8|
|12|9|
|13|9|
|14|10|
|15|11|
|16|11|
|17|12|
|18|13|
|19|14|
|20|14|
|21|15|
|22|15|
