# Style your own data

In this Example we create our own custom style to make the data look we want. To keep it simple
we'll be styling a line feature to be drawn with a light yellow, with yellow-grey dashed border and the `name` property shown as text inside the line.

To do this, we define our own style group for the line (called `lineStyle` in the code). As you can
see, it get's build up by the following drawing primitives provided inside the array:

* 18px wide Yellow line
* 18px wide Dark Grey dashed line (12px stroke, 10px gap)
* thinner 10px Light Yellow line
* text from the GeoJSON at the path `properties.name` in light grey

Note that the `zIndex` property for each entry in the array defines the drawing order from
bottom to top (ascending values)  - not the order in which the array was defined.

To find out more about all the possible styling options, have a look at the
[documentation for the `TileLayer.Style`](https://xyz.api.here.com/maps/latest/documentation/here.xyz.maps.layers.TileLayer.Style.html)

```javascript
  // customize the styles that can be used
  style:{
    styleGroups: {
      lineStyle: [
        {zIndex:0, type:"Line", stroke:"#E5B50B", strokeWidth:18, "strokeLinecap": "butt"},
        {zIndex:1, type:"Line", stroke:"#1F1A00", strokeWidth:18, "strokeLinecap": "butt", 'strokeDasharray': [12,10]},
        {zIndex:2, type:"Line", stroke:"#F7FABF", strokeWidth:10},
        {zIndex:3, type:"Text", textRef:"properties.name", fill:"#3D272B"}
      ]
    },
    // decide per feature which style to use
    assign: function(feature, zoomlevel){
      return "linkStyle";
  }
```

To be as dynamic as possible when it comes to styling there is the `assign(feature, zoomlevel)` function which you need to implement. This will decide which of the defined styles should be used for a given feature and zoom level.

Keep in mind that this function will be called for **each** feature that will be rendered, so try
to be mindful not to have too time-consuming logic there. It is expected to return a
string that is the key to your `styleGroup` definition to use for the feature.
