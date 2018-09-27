# GeoJSON Basics

GeoJSON is an open standardized format for geospatial data based on the JavaScript Object Notation (JSON) used to encode geographic data structures. This section is based on [RFC 7946](https://tools.ietf.org/html/rfc7946>).

GeoJSON is a textual representation of geographical data and its nonspatial attributes.
A GeoJSON document contains one object with can be a *Feature*, a *FeatureCollection* or a *Geometry*. The object has to have a property *type* which is one of the following:

## Point

The *coordinates* array of the **Point** object usually contains two to three dimensions representing longitude, latitude and possibly altitude in this order.

### Example Point

```JSON
{
    "type": "Point",
    "coordinates": [50.0, 50.0]
}
```

[View this **Point** on a map](http://geojson.tools/index.html?url=data:text/json,{%22type%22:%20%22Point%22,%22coordinates%22:%20[50.0,%2050.0]})

## MultiPoint

Each entry in the *coordinates* array is a **Point** *coordinates* array.

### Example MultiPoint

```JSON
{
   "type": "MultiPoint",
   "coordinates": [
       [50.0, 0.0], [51.0, 1.0]
   ]
}
```

[View this **MultiPoint** on a map](http://geojson.tools/index.html?url=data:text/json,{%22type%22:%22MultiPoint%22,%22coordinates%22:[[50.0,0.0],[51.0,1.0]]})

## LineString

A **LineString** *coordinates* array consists of two or more **Point** *coordinates* arrays.

### Example LineString

```JSON
{
   "type": "LineString",
   "coordinates": [
       [50.0, 0.0], [51.0, 1.0]
   ]
}
```

[View this **LineString** on a map](http://geojson.tools/index.html?url=data:text/json,{%22type%22:%20%22LineString%22,%22coordinates%22:%20[[50.0, 0.0],%20[51.0, 1.0]]})

## MultiLineString

As the name suggests, a **MultiLineString** *coordinates* array contains **LineString** *coordinates* arrays.

### Example MultiLineString

```JSON
{
   "type": "MultiLineString",
   "coordinates": [
       [ [50.0, 0.0], [51.0, 1.0] ],
       [ [52.0, 2.0], [53.0, 3.0] ]
   ]
}
```

[View this **MultiLineString** on a map](http://geojson.tools/index.html?url=data:text/json,{%22type%22:%20%22MultiLineString%22,%22coordinates%22:%20[[[50.0, 0.0],%20[51.0, 1.0]],[[52.0, 2.0],%20[53.0, 3.0]]]})

## Polygon

Each element of a **Polygon** *coordinates* array has to be a special **LineString** *coordinates* array. This special kind is called a ***'linear ring'*** in the RFC specification. In a ***'linear ring'*** the first and last elements in the *coordinates* array are the same.

If the **Polygon** contains more than one ***'linear ring'***, that is a shape with holes, the first ring must describe the exterior ring, the following the holes.

### Example Polygon

```JSON
{
   "type": "Polygon",
   "coordinates": [
       [ [50.0, 0.0], [51.0, 0.0], [51.0, 1.0], [50.0, 1.0], [50.0, 0.0] ]
   ]
}
```

[View this **Polygon** on map](http://geojson.tools/index.html?url=data:text/json,{%22type%22:%20%22Polygon%22,%22coordinates%22:%20[[ [50.0, 0.0], [51.0, 0.0], [51.0, 1.0], [50.0, 1.0], [50.0, 0.0] ]]})

## MultiPolygon

In a **MultiPolygon**, each of the elements of the coordinates array is defined as a **Polygon** above.

### Example MultiPolygon

```JSON
{
   "type": "MultiPolygon",
   "coordinates": [
       [
           [ [52.0, 2.0], [53.0, 2.0], [53.0, 3.0], [52.0, 3.0], [52.0, 2.0] ]
       ],
       [
           [ [50.0, 0.0], [51.0, 0.0], [51.0, 1.0], [50.0, 1.0], [50.0, 0.0] ],
           [ [50.2, 0.2], [50.8, 0.2], [50.8, 0.8], [50.2, 0.8], [50.2, 0.2] ]
       ]
   ]
}
```

[View this **MultiPolygon** on a map](http://geojson.tools/index.html?url=data:text/json,{%22type%22:%22MultiPolygon%22,%22coordinates%22:[[[[52.0,2.0],[53.0,2.0],[53.0,3.0],[52.0,3.0],[52.0,2.0]]],[[[50.0,0.0],[51.0,0.0],[51.0,1.0],[50.0,1.0],[50.0,0.0]],[[50.2,0.2],[50.8,0.2],[50.8,0.8],[50.2,0.8],[50.2,0.2]]]]})

## Feature

A **Feature** object contains two members besides the *type* property: *geometry* and *properties*

The *geometry* can be any of the aforementioned types or a GeoJSON null value.

The value of *properties* can be any GeoJSON object or a GeoJSON null value.

### Example Feature

```JSON
{
   "type": "Feature",
   "geometry": {
       "type": "LineString",
       "coordinates": [
           [-50.0,-10.0],[50.0,-10.0],[50.0,10.0],[-50.0,10.0]
       ]
   },
   "properties": {
       "prop0": "value0",
       "prop1": "value1"
   }
}
```

[View this **Feature** on a map](http://geojson.tools/index.html?url=data:text/json,{%22type%22:%22Feature%22,%22geometry%22:{%22type%22:%22LineString%22,%22coordinates%22:[[-50.0,-10.0],[50.0,-10.0],[50.0,10.0],[-50.0,0.0]]},%22properties%22:{%22prop0%22:%22value0%22,%22prop1%22:%22value1%22}})

## FeatureCollection

A **FeatureCollection** contains an array of **Features**, contained in a member called unsurprisingly *features*.

### Example FeatureCollection

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
        "{spaceI}d@ns:com:here:xyz": {
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

[View this **FeatureCollection** on a map](http://geojson.tools/index.html?url=data:text/json,{%22type%22:%22FeatureCollection%22,%22features%22:[{%22type%22:%22Feature%22,%22geometry%22:{%22type%22:%22Point%22,%22coordinates%22:[-2.960847,53.430828]},%22properties%22:{%22{spaceId}@ns:com:here:xyz%22:{%22tags%22:[%22football%22,%22stadium%22]},%22name%22:%22Anfield%22,%22amenity%22:%22Football Stadium%22,%22capacity%22:54074,%22popupContent%22:%22Home of Liverpool Football Club%22}}]})

## GeometryCollection

A **GeometryCollection** is a collection of zero or more geometry objects like the ones above in an array member called *geometries*

!!! Warning "GeometryCollection is not supported by HERE XYZ Hub. It is just included here for completeness."

### Example GeometryCollection

```JSON
{
   "type": "GeometryCollection",
   "geometries": [
       {
           "type": "Point",
           "coordinates": [50.0, 0.0]
       },
       {
           "type": "LineString",
           "coordinates": [
               [51.0, 0.0], [52.0, 1.0]
           ]
       }
   ]
}
```
