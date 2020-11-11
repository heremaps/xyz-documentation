# Optimized modes for tile visualization on lower zoom level

On lower zoom levels, to reduce the payload, tiled data can be optimized in size.
This will result in better performace for rendering clients. 
This affects the amount of features, their attributes and coordinates.

All described parameters and values are related to tile requests.
## mode=viz

* reduce size of single features<br>
default, every feature will contain an empty feature.properties value. (e.g. properties:{} )<br>
To get specific properties attributes, they must be explicit specified with tile parameter "selection=".
(e.g. "http://....?mode=viz&selection=p.firstAttrib,p.secondAttrib,...p.lastAttrib )
For parameter "selection=" the special values "*", for all attributes, and "none" can be used.

* reduce coordinates of geometry<br>
depending on requested tile level the coordinates of the geometries (except for point objects) are reduced, to at most one coordinate per pixel (512 pixelsize per tile)

* reduce amount of features per tile<br>
  default, a sampling threshold of max 30k objects, for a single tile on 3x3 tile grid is used (s.b)<br>
  The threshold can be controled with the additional parameter vizSampling (s.b.)

### vizSampling={value}

In order to reduce the amount of data returned, only a evenly distributed sample of data can be requested. 
Parameter vizSampling sets the wanted threshold for the 3x3 tile grid, surrounding the requested tile.
E.g. setting "vizSampling=med" when requsting a specifc tile, will calculate the samplinratio such that, the "heaviest" tile in the related 3x3 grid will contain at most 30k objects.

|value | threshold |
|---|---|
| low  | 70k |
| med  | 30k (default) |
| high | 15k |
| off  |  |

#### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeaturesByTile)*

```HTTP
GET /spaces/{spaceId}/tile/{type}/{tileId}?mode=viz&vizSampling=med&selection=*
```

#### Response

