## Creating hexbins with the HERE CLI

The `hexbin` command in the HERE CLI lets you quickly create hexbins and their centroids from large, dense point datasets in your XYZ spaces. Hexbins can be useful for data analysis, and can also allow you to render datasets that are too large to effectively view at low zoom levels.

## Getting started

If you don't already have a set of points in an XYZ space, you can upload them using the CLI.

	here xyz upload -f your.geojson
	
(You can also use a CSV with point coordinates or shapefiles.) 

If you don't have data handy, you can use this CSV of [bicycle parking in San Francisco](https://data.sfgov.org/Transportation/Bicycle-Parking/hn4j-6fx5). 

	here xyz upload -f "https://data.sfgov.org/api/views/hn4j-6fx5/rows.csv"
	
(Note: some of the features in this file do not have any coordinates -- the HERE CLI will report these as errors.)
	
Copy the space ID and preview using geojson.tools or XYZ Space Invader (it will be easier to view larger data sets with XYZ Space Invader.)

	here xyz show spaceID -w 
	
	here xyz show spaceID -v 

### Creating hexbins

Hexbins have a width in meters. You can set this manually using `-c`, but you also can use `-z` to generate a hexbin grid that fits well with a particular slippy map zoom level.

	here xyz hexbin spaceID -c 100
	
	here xyz hexbin spaceID -z 10
	
You can also define multiple zoom levels, or a range:

	here xyz hexbin spaceID -z 10-13
	here xyz hexbin spaceID -z 8,10,12
	here xyz hexbin spaceID -c 100,200,400,800
	
For reference, here's a rough guide of zoom levels and admin hierarchies:

	city: zoom 10-13
	region: zoom 7-10
	country: zoom 4-7
	world: zoom 1-4

XYZ Hexbins will generate a hexagonal grid for each zoom level (or width) that you selected. It will then count the points that fall within each hexagon. It also tracks the maximum count seen in that grid, and after the pass is completed, it writes that value to each hexbin. An "occupancy" rate is then calculated for each hexbin relative to the rest of that grid, and a scaled `hsla` color value is written to the feature for display convenience (blue is low, red is high).


### Using tags with hexbin data

The CLI also creates centroids for each hexbin. This is useful for general display and for labels. You can use XYZ tags to pull these out separately.

If we generate hexbins for XYZ space containing the bike parking locations:

	here xyz hexbin spaceID -z 9-13
	
these tags can be used to pull out the hexbins and centroids for each zoom level:

	zoom9
	zoom9_hexbin
	zoom9_centroid
	zoom10
	zoom10_hexbin
	zoom10_centroid
	...

After you generate the hexbins from an XYZ space, you can view them with any GeoJSON viewer. If you want to view just the zoom 11 hexbins, you can use the `-t` option with view:

	here xyz show spaceID -w -t zoom11_hexbin
	
For convenience, here is a link to that data:

http://geojson.tools/index.html?url=https://xyz.api.here.com/hub/spaces/ZGAzaLaA/search?limit=5000&clientId=cli&tags=zoom11_hexbin&access_token=APwC9OKv8ww_zMGWqPTSQdg

![xyz-hexbin-geojson-tools](https://github.com/heremaps/xyz-documentation/blob/master/docs/assets/images/xyz-hexbin-geojson-tools.png)

### Data contained in XYZ Hexbins

Hexbin features contain various values that can help with analysis and visualization:
- `count`: the number of points in a hexbin
- `maxCount`: the largest number of points in any hexbin across that particular zoom level or cell width
- `occupancy`: `count/maxCount`, how "full" that hexbin is compared to other hexbins across that particular zoom level or cell width
- `color`: an `hsla` color value that correlates to that hexbin's relative occupancy (red = "full", green = "average", blue = "empty
- `centroid`: the centroid of the hexbin (useful for label placement -- the centroid is also written as a separate feature)

```
      "properties": {
        "color": "hsla(0, 100%, 50%,0.51)",
        "count": 468,
        "maxCount": 468,
        "occupancy": 1,
      },
      ...
      "properties": {
        "color": "hsla(81, 100%, 50%,0.51)",
        "count": 279,
        "maxCount": 468,
        "occupancy": 0.5961538461538461
      },
      ...
      "properties": {
        "color": "hsla(197, 100%, 50%,0.51)",
        "count": 6,
        "maxCount": 468,
        "occupancy": 0.01282051282051282...
      }
```




You can also open the hexbin space in XYZ Space Invader and select zoom levels from the tag list.

	here xyz show spaceID -v 
	
In XYZ Space Invader, you can select properties and choose data-driven color palettes, and change the basemaps to suit the data.
	
If you do not choose a tag with `-t` in the CLI, you can select a zoom level using the tags in the right pane.


![xyz-hexbin-space-invader](https://github.com/heremaps/xyz-documentation/blob/master/docs/assets/images/xyz-hexbin-space-invader.png)

https://s3.amazonaws.com/xyz-demo/scenes/xyz_tangram/index.html?space=ZGAzaLaA&token=APwC9OKv8ww_zMGWqPTSQdg&basemap=xyz-pixel-dark&buildings=1&label=undefined&colors=range&points=2&lines=0&outlines=0&highlight=0&places=1&roads=1&water=0&tags=zoom12_centroid&property=count&palette=colorBrewerYlOrRd&paletteFlip=true&sort=values&hideOutliers=false#12.858333333333318/37.7474/-122.4452

Click for more examples on how to use [XYZ Space Invader](https://github.com/heremaps/xyz-documentation/blob/master/docs/space-invader/tutorial.md).

## Advanced options

### Subgroups

Values of properties of a feature can also be counted with `here xyx hexbin -p propertyname`. 

	here xyz hexbin spaceID -p PLACEMENT
	
This would count the unique values of the PLACEMENT property in each feature, track the maximum, and generate an occupancy value to be used in data visualization.

### Updates

XYZ tracks the source space of a hexbin space, and vice versa. If you add zoom levels or update hexbins, XYZ will add to or edit any existing hexbin spaces for a source space.

You can see in the description of the space, using `here xyz config spaceID -r`. Hexbin zoom levels are also tracked.

### Dynamic zoom

Since each zoom level can have its own hexbins and centroids, you can dynamically select data appropriate for the zoom level of a map using tags. Here's an example of centroids and hexbins for tornados in the United States from 1950 to 2017.

https://burritojustice.github.io/noaa_historic_tornadoes/

	

