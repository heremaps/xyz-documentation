# Hexbins

## Creating hexbins with the HERE CLI

The `hexbin` command in the HERE CLI lets you easily create hexbins and their centroids from large, dense point datasets in your XYZ spaces. Hexbins can be useful for data analysis, and can also allow you to visualize datasets that are too large to effectively view at low (regional or national) map zoom levels.

## Getting started

If you don't already have a set of points in a Data Hub space, you can upload them using the CLI.

	here xyz upload -f your.geojson
	smap
(You can also use a CSV with point coordinates or shapefiles.) 

If you don't have data handy, you can use this CSV of [bicycle parking in San Francisco](https://data.sfgov.org/Transportation/Bicycle-Parking/hn4j-6fx5). 

	here xyz upload -f "https://data.sfgov.org/api/views/hn4j-6fx5/rows.csv"
	
	
You'll be prompted to enter a title and description, and Data Hub will generate a unique ID for your dataset. Copy this as you'll need it to access the data and generated hexbins.

!!! note 
    Some of the features in this file do not have any coordinates -- the HERE CLI will report these as errors.
    
	
After the upload finishes, you can preview the map using geojson.tools 

    	here xyz show spaceID -w 

or [Data Hub Space Invader](../space-invader/tutorial.md). (Note it will be easier to view data sets larger than a few hundred points using Data Hub Space Invader.)
	
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

Data Hub Hexbins will generate a hexagonal grid for each zoom level (or width) that you selected. It will then count the points that fall within each hexagon, and save that to the properties of each hexbin. 

It also tracks the maximum count seen in that grid, and after the pass is completed, it writes that `maxCount` value to each hexbin. An `occupancy` rate is then calculated for each hexbin relative to the rest of that grid, and a scaled `hsla` color value is written to the feature for display convenience (blue is low, red is high).


### Using tags with hexbin data

If we generate hexbins for Data Hub space containing the bike parking locations...

	here xyz hexbin spaceID -z 9-13
	
the CLI will generate tags so you can pull out the hexbins and centroids for each zoom level:

	zoom9
	zoom9_hexbin
	zoom9_centroid
	zoom10
	zoom10_hexbin
	zoom10_centroid
	...
	
	
Note tha the CLI also creates centroids for each hexbin. This is useful for an alternate display method as well as for labels. You can use Data Hub tags to pull these out separately from the hexbins.

After you generate the hexbins from a Data Hub space, you can view them with any GeoJSON viewer. If you want to view just the zoom 11 hexbins, you can use the `-t` option with view:

	here xyz show spaceID -w -t zoom11_hexbin
	
For convenience, here is a link to that data. Note the `&tags=zoom11_hexbin` -- that tells the Data Hub API to only return features with that tag, and thus that zoom level.

http://geojson.tools/index.html?url=https://xyz.api.here.com/hub/spaces/ZGAzaLaA/search?limit=5000&clientId=cli&tags=zoom11_hexbin&access_token=APwC9OKv8ww_zMGWqPTSQdg

![xyz-hexbin-geojson-tools](https://github.com/heremaps/xyz-documentation/blob/master/docs/assets/images/xyz-hexbin-geojson-tools.png)

### Data contained in Data Hub Hexbins

Hexbin features contain various values that can help with analysis and visualization:
- `count`: the number of points in a hexbin
- `maxCount`: the largest number of points in any hexbin across that particular zoom level or cell width
- `occupancy`: how "full" that hexbin is compared to other hexbins across that particular zoom level or cell width (`count` divided by `maxCount`)
- `color`: an `hsla` color value that correlates to that hexbin's relative occupancy (red = "high", green = "average", blue = "low", provided for display convience
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

You can also open the hexbin space in Data Hub Space Invader and select zoom levels from the tag list.

	here xyz show spaceID -v 
	
In Data Hub Space Invader, you can select properties and choose data-driven color palettes, and change the basemaps to suit the data.
	
If you do not choose a tag with `-t` in the CLI, you can select a zoom level using the tags in the right pane.


![xyz-hexbin-space-invader](https://github.com/heremaps/xyz-documentation/blob/master/docs/assets/images/xyz-hexbin-space-invader.png)

https://s3.amazonaws.com/xyz-demo/scenes/xyz_tangram/index.html?space=ZGAzaLaA&token=APwC9OKv8ww_zMGWqPTSQdg&basemap=xyz-pixel-dark&buildings=1&label=undefined&colors=range&points=2&lines=0&outlines=0&highlight=0&places=1&roads=1&water=0&tags=zoom12_centroid&property=count&palette=colorBrewerYlOrRd&paletteFlip=true&sort=values&hideOutliers=false#12.858333333333318/37.7474/-122.4452

Click for more examples on how to use [Data Hub Space Invader](../space-invader/tutorial.md).

## Advanced options

### Subgroups

Values of properties of a feature can also be counted with `here xyx hexbin -p propertyname`. 

	here xyz hexbin spaceID -p street_type
	
This would count the unique values of the `street_type` property in each feature, track the maximum, and generate an occupancy value to be used in data visualization. Data Hub Space Invader can use that value to dynamically generate a color based on that hexbin's subgroup value compared to other hexbin's subgroup values.

Here's an example [showing street types in San Francisco](https://s3.amazonaws.com/xyz-demo/scenes/xyz_tangram/index.html?space=qAtS3e8G&token=AFbjoHrBlTB2K5_gqvcP_S8&basemap=xyz-pixel-dark&buildings=1&label=undefined&colors=range&points=2&lines=0&outlines=1&highlight=0&places=1&roads=1&water=0&tags=zoom12_hexbin&property=subcount.AVE.count&palette=colorBrewerYlGnBu&paletteFlip=true&rangeFilter=0&sort=values&hideOutliers=true#13.166666666666664/37.7568/-122.4384), where street type was chosen as a subgroup. `subcount.AVE` can be used to see where Avenue is the most common street type. You can click on `subcount.ST.count` to see the streets of San Francisco.

![xyz-hexbin-space-invader](../assets/images/hexbin_subgroup.png)

For reference, [here is a sample of a subcount object](http://geojson.tools/index.html?url=https://xyz.api.here.com/hub/spaces/qAtS3e8G/search?limit=5000&clientId=cli&tags=zoom10_centroid&access_token=AFbjoHrBlTB2K5_gqvcP_S8):

```
      "properties": {
        "count": 468,
        "maxCount": 468,
        "subcount": {
          "ST": {
            "color": "hsla(0, 100%, 50%,0.51)",
            "count": 324,
            "maxCount": 324,
            "occupancy": 1
          },
          "ALY": {
            "color": "hsla(0, 100%, 50%,0.51)",
            "count": 16,
            "maxCount": 16,
            "occupancy": 1
          },
          "AVE": {
            "color": "hsla(107, 100%, 50%,0.51)",
            "count": 57,
            "maxCount": 122,
            "occupancy": 0.4672131147540984
          },
```

### Sum and average

If a point feature has a quantitative property, you can use `-a` to add it up within each hexbin, as well as calculate an average. These values, along with `maxSum`, is recorded in a `sum` object within each hexbin.

```
        "sum": {
          "sum": 4071,
          "maxSum": 5117,
          "average": 8.698717948717949,
          "property_name": "incidents"
        }
```

Data Hub Space Invader can compare and color these values across the hexbin grid. [Here we see the aggregate values of Minneapolis property values summed within hexbin centroids](https://s3.amazonaws.com/xyz-demo/scenes/xyz_tangram/index.html?space=ZL228Jrk&token=AFbjoHrBlTB2K5_gqvcP_S8&basemap=xyz-pixel-dark&buildings=1&label=undefined&colors=range&points=0&lines=0&outlines=0&highlight=0&places=1&roads=1&water=0&tags=zoom13_centroid&property=sum&palette=colorBrewerYlOrRd&paletteFlip=true&rangeFilter=4&sort=values&hideOutliers=false#12.370833333333346/44.9662/-93.2612).

![minneapolis](https://github.com/heremaps/xyz-documentation/blob/master/docs/assets/images/hexbin_sum.png)



### Updates

Data Hub tracks the source space of a hexbin space, and vice versa. If you add zoom levels or update hexbins, Data Hub will add to or edit any existing hexbin spaces for a source space.

You can see in the description of the space, using `here xyz config spaceID -r`. Hexbin zoom levels are also tracked.

### Dynamic zoom

Since each zoom level can have its own hexbins and centroids, you can dynamically select data appropriate for the zoom level of a map using tags. Here's an example of centroids and hexbins for tornados in the United States from 1950 to 2017.

https://burritojustice.github.io/noaa_historic_tornadoes/


# Shapefiles

## Importing shapefiles into Data Hub

[Shapefiles](https://en.wikipedia.org/wiki/Shapefile) are a proprietary but common geospatial file format developed by ESRI. It is frequently used by governments to store geospatial data. 

As of version 1.1 of the HERE CLI, most shapefiles can be easily uploaded into a Data Hub Space. 

	here xyz upload -f my_shapefile.shp

The CLI inspects the CRS and projection data in the `.prj` file normally found in the unzipped shapefile directory and will attempt to convert it to WGS84. If the CLI returns an error, the shapefile will require extra steps before you can bring it into Data Hub.  

In this tutorial, we'll cover what you need to do to successfully import shapefiles, along with special steps using other open source tools for those trickier ones.

This document assumes:

- you have already have a free [HERE developer account](https://developer.here.com/)
- you have installed the [HERE Data Hub CLI](https://developer.here.com/tutorials/install-here-cli/)
- you have reviewed the [Using the CLI](https://developer.here.com/tutorials/using-the-xyz-cli/) codelab
 
You should also install
- [mapshaper](https://github.com/mbloch/mapshaper)
- [QGIS](https://www.qgis.org/) and the [HERE Data Hub QGIS plugin](https://plugins.qgis.org/plugins/XYZHubConnector/)

## Standard shapefile upload via the HERE Data Hub CLI

Unlike a GeoJSON file, a shapefile is made up of a number of separate files. Shapefiles on the internet are usually zipped, but once uncompressed you will see a number of files with the same name but different extensions. Some of the more important ones are:

- `.shp` - contains the geometries of the features (points, lines, polygons)
- `.dbf` - contains the attributes for the features 
- `.prj` - contains information about the projection and coordinate reference system (CRS)

If the shapefile is under 200MB, you should be able to upload it using the HERE Data Hub CLI. 

In the terminal, `cd` to the unzipped shapefile directory, and type

	here xyz upload space_id -f my_shapefile.shp
	
The CLI will look for `my_shapefile.dbf` and other files in the specified directory. (If it is missing, no attributes of the geometries will be imported.)

Note that you can use `-a` to select attributes of features to convert into tags, which will let you filter features server-side when you access the Data Hub API.

## Advanced shapefile upload

Shapefiles are an infinitely variable format, and there will be cases where you may need to manipulate or modify the data in order to import it into your Data Hub space. You can do this with other open-source geospatial tools, specifically `mapshaper` and QGIS.

### mapshaper

`mapshaper` is a powerful command-line tool for editing and manipulating geospatial data in a variety of common formats.

	https://github.com/mbloch/mapshaper
	https://github.com/mbloch/mapshaper/wiki/Command-Reference
	
You can install it using `npm`:
	
	npm install -g mapshaper
	
Note that `mapshaper` can modify shapefiles directly, or convert shapefiles into GeoJSON. Converting to GeoJSON will give you more options and faster uploads when bringing the data into Data Hub. The [mapshaper documentation](https://github.com/mbloch/mapshaper/wiki/Command-Reference) provides a wide variety of options, but a simple conversion command is:

	mapshaper my_geodata.shp -o my_geodata.geojson
	here xyz upload -f my_geodata.geojson -a
	
`-a` lets you interactively pick property values to convert into tags. You can use `-s` to stream the file and upload it much more quickly, but in this case you will need to specify the property keys with `-p`

	here xyz upload -f my_geodata.geojson -p property_name -s
	
Depending on the size of the shapefile you may be able to pipe the geojson from `mapshaper` directly to the Data Hub HERE CLI, using the `-` option in `mapshaper`: 

	mapshaper my_geodata.shp -o format=geojson - | here xyz upload spaceID -a -t specific_tag
	
_Note: While you normally can use `upload` without specifying a Data Hub Space ID, you need to do so when piping._

You can also stream it, which will upload your data much more quickly:
	
	mapshaper my_geodata.shp -o format=geojson - | here xyz upload spaceID -p property_name -t specific_tag -s
	
(If you see unusual errors when piping from `mapshaper` to Data Hub, you may have more success keeping the conversion and uploading as separate steps.)

Note that you can also run `mapshaper` as a web app, though there may be limits on file sizes.

	http://mapshaper.org


### HERE Data Hub QGIS plugin

QGIS is an open-source desktop GIS tool that lets you edit, visualize, manage, analyze and convert geospatial data. You can upload and download data from your Data Hub spaces using the [HERE Data Hub QGIS plugin](https://plugins.qgis.org/plugins/XYZHubConnector/). (The plugin is also available [on Github](https://github.com/heremaps/xyz-qgis-plugin).)

You can install the HERE Data Hub QGIS plugin from within QGIS Plugin search tool if you have the "show experimental plugins" option checked in the plugin console settings.

![experimental](../assets/images/qgis_plugin_experimental.png)

You can easily open almost any shapefile in QGIS, at which point you can save it to your Data Hub spaces using the HERE Data Hub QGIS plugin, or export it as GeoJSON to the desktop to use the HERE Data Hub CLI streaming upload options.


## Large individual features

Some shapefiles may contain very large and extremely detailed individual lines or polygons. (Coastlines are a common example.) If a single feature is greater than 10-20MB, you may see `400` or `413` http errors when you try to upload the shapefile. In many cases, this level of detail is unnecessary for web mapping. If so, you can try to simplify the feature using `mapshaper` or QGIS. You may also want to adjust HERE Data Hub CLI upload parameters so less data is sent in each API request.

### Adjusting 'chunk' parameters

In order to optimize upload speed, the CLI "chunks" features together and then sends the chunk to the API. There are typically 200 features per chunk. While a large feature may be small enough to be uploaded, when combined with other features, the chunk may be too large for the API.

You can adjust the chunk size using `-c` -- in this example, the CLI will upload 100 features per API request:
	
	here xyz upload spaceID -f large_features.shapefile -c 100

Depending on the size of the feature, you may want to try `c -10` (ten per request) or even `c -1` (which would load one feature at a time).

### Simplifying with mapshaper

You can simplify lines and polygons in shapefiles using `-simplify`.

	mapshaper very_large_features.shp -simplify dp 20% -o simplified_features.geojson
	
Depending on the zoom level and extent your web map, you can also try `10%`, `5%`, and `1%`.
	
More information on simplification is available here: https://github.com/mbloch/mapshaper/wiki/Command-Reference#-simplify

As previously mentioned, for smaller shapefiles you can pipe output from `mapshaper` directly to the HERE Data Hub CLI, accelerating your TTM (Time To Map).

	mapshaper big_shapefile.shp -o format=geojson - | here xyz upload spaceID -p property_name -t specific_tag -s
	
### QGIS

- open the shapefile in QGIS
- choose Vector -> Geometry Tools -> Simplify
- save the simplified data to a new Data Hub space using the HERE Data Hub plugin

Note that the Simplify tool works in decimal degrees, and the default is 1 degree, which is probably not what you want. Useful values depend on the extent and zoom levels of your map, but `0.01`, `0.001`, `0.0001`, and `0.00001` are interesting values.


## Very large shapefiles (> 200MB)

The HERE Data Hub CLI will attempt to load the entire shapefile into memory before uploading it to the API. This will generally work for shapefiles up to 200MB, but you will start to see Node.js memory errors beyond that.

While GeoJSON and CSVs can be streamed via the `upload -s` option, this option is not yet available for shapefiles. You will have the most success converting the shapefile to GeoJSON and then uploading to Data Hub.

	mapshaper big_data.shp -o format=geojson big_data.geojson
	here xyz upload spaceID -f big_data.geojson -s
	
_Note that `-a` is not available when `-s` is used, but you can still specify properties to convert into tags using `-p`._

You can also open the very large shapefile in QGIS and save directly to a Data Hub space using the Data Hub QGIS plugin, though this will be slower than using the CLI streaming feature as the QGIS plugin is not multi-threaded.
	
## Projections and CRS (Coordinate Reference Systems)

Just like standards, the beauty of projections is there are so many to choose from. GeoJSON expects points to be projected in Web Mercator (WGS84/EPSG:4326). Many shapefiles are in different projections, or use local projections without lat/lon coordinates (i.e. state plane). The CLI will inspect the .prj file and attempt to convert it. If it is an uncommon projection, you may see errors, but it is easy to get `mapshaper` to try to convert into GeoJSON-friendly coordinates.

	mapshaper different_projection.shp -proj wgs84 -o format=geojson - | here xyz upload spaceID -p property_name -t specific_tag -s
	
If you see any `node.js` memory errors, you can break it up into two steps:

	mapshaper different_projection.shp -proj wgs84 -o format=geojson different_projection.geojson
	here xyz upload spaceID -f different_projection.geojson
	
If you continue to see errors, you may want to try opening the shapefile in QGIS or use GDAL's `ogr2ogr` conversion tools.



