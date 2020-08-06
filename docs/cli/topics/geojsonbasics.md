## GeoJSON Basics

GeoJSON is an open standardized format for geodata based on the Javascript Object Notation (JSON) used to encode geographic data structures. This section is based on [RFC 7946](https://tools.ietf.org/html/rfc7946>).

GeoJSON is a textual representation of geographical data and its non-spatial attributes.
A GeoJSON document contains one object with can be a *Feature*, a *FeatureCollection* or a *Geometry*. The object has to have a property *type* which is one of the following:

### Point

The *coordinates* array of the **Point** object usually contains two to three dimensions representing longitude, latitude and possibly elevation in this order.

#### Example Point

```JSON
{
    "type": "Point",
    "coordinates": [8.53362,50.16192]
}
```

[View this **Point** on a map](http://geojson.tools/index.html?url=data:text/json,%22type%22:%20%22Point%22,%22coordinates%22:%20[8.53362,50.16192])

### MultiPoint

Each entry in the *coordinates* array is a **Point** *coordinates* array.

#### Example MultiPoint

```JSON
{
   "type": "MultiPoint",
   "coordinates": [
       [13.35,52.5145], [13.377,52.5162]
   ]
}
```

[View this **MultiPoint** on a map](http://geojson.tools/index.html?url=data:text/json,%22type%22:%22MultiPoint%22,%22coordinates%22:[[13.35,52.5145],[13.377,52.5162]])

### LineString

A **LineString** *coordinates* array consists of two or more **Point** *coordinates* arrays.

#### Example LineString

```JSON
{
   "type": "LineString",
   "coordinates": [
       [13.35,52.5145], [13.377,52.5162]
   ]
}
```

[View this **LineString** on a map](http://geojson.tools/index.html?url=data:text/json,%22type%22:%20%22LineString%22,%22coordinates%22:%20[[13.35,52.5145],%20[13.377,52.5162]])

### MultiLineString

As the name suggests, a **MultiLineString** *coordinates* array contains **LineString** *coordinates* arrays.

#### Example MultiLineString

```JSON
{
        "type": "MultiLineString",
        "coordinates": [
          [
            [78.0422,27.1744],[78.0422,27.1733]
          ],
          [
            [78.0422,27.1730],[78.0422,27.1719]
          ]
        ]
      }
```

[View this **MultiLineString** on a map](http://geojson.tools/index.html?url=data:text/json,%22type%22:%20%22MultiLineString%22,%22coordinates%22:%20[[[78.0422,27.1744],[78.0422,27.1733]],[[78.0422,27.1730],[78.0422,27.1719]]])

### Polygon

Each element of a **Polygon** *coordinates* array has to be a special **LineString** *coordinates* array. This special kind is called a ***'linear ring'*** in the RFC specification. In a ***'linear ring'*** the first and last elements in the *coordinates* array are the same.

If the **Polygon** contains more than one ***'linear ring'***, that is a shape with holes, the first ring must describe the exterior ring, the following the holes. The coordinates of the exterior ring must be listed counterclockwise, the coordinates of the hole(s) clockwise.

#### Example Polygon

```JSON
{
  "type": "Polygon",
  "coordinates": [
    [
      [-77.0578,38.87254],[-77.0584,38.87],[-77.0555,38.8688],[-77.0531,38.8706],[-77.0546,38.8729],[-77.0578,38.87254]
    ],
    [
      [-77.0567,38.8715],[-77.0555,38.8717],[-77.0549,38.8708],[-77.0558,38.8702],[-77.0569,38.8706],[-77.0567,38.8715]
    ]
  ]
}
```

[View this **Polygon** on map](http://geojson.tools/index.html?url=data:text/json,%22type%22:%20%22Polygon%22,%22coordinates%22:%20[[[-77.0578,38.87254],[-77.0584,38.8700],[-77.0555,38.8688],[-77.0531,38.8706],[-77.0546,38.8729],[-77.0578,38.87254]],[[-77.0567,38.8715],[-77.0555,38.8717],[-77.0549,38.8708],[-77.0558,38.8702],[-77.0569,38.8706],[-77.0567,38.8715]]])

### Multipolygon

In a **Multipolygon**, each of the elements of the coordinates array is defined as a **Polygon** above.

#### Example Multipolygon

```JSON
{
   "type": "MultiPolygon",
        "coordinates": [
          [
             [[31.1330,29.9780],[31.1354,29.9780],[31.1354,29.9801],[31.1330,29.9801],[31.1330,29.9780]],
             [[31.1296,29.9750],[31.1319,29.9750],[31.1319,29.9769],[31.1296,29.9769],[31.1296,29.9750]],
             [[31.1277,29.9720],[31.1287,29.9720],[31.1287,29.9729],[31.1277,29.9729],[31.1277,29.9720]]
          ]
        ]
}
```

[View this **MultiPolygon** on a map](http://geojson.tools/index.html?url=data:text/json,%22type%22:%22MultiPolygon%22,%22coordinates%22:[[[[31.1330,29.9780],[31.1354,29.9780],[31.1354,29.9801],[31.1330,29.9801],[31.1330,29.9780]],[[31.1296,29.9750],[31.1319,29.9750],[31.1319,29.9769],[31.1296,29.9769],[31.1296,29.9750]],[[31.1277,29.9720],[31.1287,29.9720],[31.1287,29.9729],[31.1277,29.9729],[31.1277,29.9720]]]])

### Feature

A **Feature** object contains two members besides the *type* property: *geometry* and *properties*

The *geometry* can be any of the aforementioned types or a GeoJSON null value.

The value of *properties* can be any GeoJSON object or a GeoJSON null value.

#### Example Feature

```JSON
{
   "type": "Feature",
   "geometry": {
       "type": "LineString",
      "coordinates": [
       [13.35,52.5145], [13.377,52.5162]
   ]
   },
   "properties": {
       "prop0": "value0",
       "prop1": "value1"
   }
}
```

[View this **Feature** on a map](http://geojson.tools/index.html?url=data:text/json,%22type%22:%22Feature%22,%22geometry%22:%22type%22:%22LineString%22,%22coordinates%22:[[13.35,52.5145],[13.377,52.5162]],%22properties%22:%22prop0%22:%22value0%22,%22prop1%22:%22value1%22)

### FeatureCollection

A **FeatureCollection** contains an array of **Features**, contained in a member called unsurprisingly *features*.

#### Example FeatureCollection

```JSON
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -2.960847,
          53.430828
        ]
      },
      "properties": {
        "{spaceId}@ns:com:here:xyz": {
          "tags": [
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

[View this **FeatureCollection** on a map](http://geojson.tools/index.html?url=data:text/json,%22type%22:%22FeatureCollection%22,%22features%22:[%22type%22:%22Feature%22,%22geometry%22:%22type%22:%22Point%22,%22coordinates%22:[-2.960847,53.430828],%22properties%22:%22spaceId@ns:com:here:xyz%22:%22tags%22:[%22football%22,%22stadium%22],%22name%22:%22Anfield%22,%22amenity%22:%22Football%20Stadium%22,%22capacity%22:54074,%22popupContent%22:%22Home%20of%20Liverpool%20Football%20Club%22])

### GeometryCollection

A **GeometryCollection** is a collection of zero or more geometry objects like the ones above in an array member called *geometries*


> #### Warning 
> GeometryCollection is not supported by HERE XYZ Hub. It is just included here for completeness.


#### Example GeometryCollection

```JSON
{
   "type": "GeometryCollection",
   "geometries": [
       {
           "type": "Point",
           "coordinates": [8.53362,50.16192]
       },
       {
           "type": "LineString",
           "coordinates": [
               [13.35,52.5145], [13.377,52.5162]
           ]
       }
   ]
}
```
