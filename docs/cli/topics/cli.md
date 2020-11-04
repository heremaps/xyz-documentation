# Commands

In this section we give you a quick overview of the most commonly used commands to interact
with XYZ Spaces from the HERE Data Hub CLI.


> #### Warning 
> Configuration is required for HERE Data Hub CLI to work.
> If you have skipped the previous section now is the time to go back and verify that
> HERE Data Hub CLI is properly configured with your developer identity. In case you are not
> certain, you can run `here configure verify` to check if your credentials are valid.
  
    
## Supported Commands

HERE Data Hub CLI is built to be modular and extensible, so it is entirely possible that when you
use the tool it has already learned a couple of new tricks.

The general structure is that you call the `here` command followed by *command*
which essentially corresponds to the API you want to interact with or get more information
from.

### Help

You can always call up help on the HERE Data Hub CLI either by not providing any parameter at all or
by using the `--help` switch.

```
here --help
```

### Configure

As explained earlier, HERE Data Hub CLI needs to know you to interact with your XYZ Spaces. You can use

```
here configure account
```
to configure with the e-mail address and password you use for your HERE account.

You can also run

```
here configure verify
```

to check if the credentials provided can be used to interact with HERE APIs.

### XYZ

The `xyz` command is used to interact with XYZ Spaces.

#### List all Spaces

To list all Spaces you have access to (with or without Schema Validation) you can use

```
here xyz list
```

#### Create a New Space

```
here xyz create -t "sample test xyz" -d "sample creation"
```

When you create a new Space, the SpaceID will be generated automatically.


> #### Hint 
> The `upload` command can also automatically generate a new space ID for you.
    


##### Options

`-t <title>` title for space

`-d <desc>` description for space


> #### Hint
> When you have many spaces, you will be glad you added meaningful titles and descriptions.


`-s <schema definition>` Applies a schema validation json file to space. The schema definition can be in the form of a web address or a local schema json file. Features that do not match this schema will not be uploaded. 


> #### Note
> This is a Pro feature that requires a license. [Learn more about XYZ Pro features here](../../xyz_pro). 
    

#### Upload/Update Data to a Space

##### Options

`-f, --file <file>`    GeoJSON, GeoJSONL, CSV or Shapefile to upload

`-c, --chunk [chunk]`  chunk size (adjusts the number of features uploaded at once)

`-t, --tags [tags]`    tags for the xyz space (used to filter data from the API)

`-x, --lon [lon]`      choose longitude CSV field name, if not well known

`-y, --lat [lat]`     latitude CSV field name, if not well known

`-z, --point [poiunt]` points field name, e.g. `(lat,lon)`

`-p, --ptag [ptag]`    property name(s) whose values will to be used to generate tags

`-i, --id [id]`        property name(s) to be used as the feature ID

`-a, --assign`         list a sample of properties, allowing you to assign fields to be selected as tags

`-u, --unique`         option to enforce uniqueness to the id by creating a hash of features to use that as id

`-o, --override`       allow duplicate features to be uploaded even if they share the same feature id

`-s, --stream`        speed up CSV and GeoJSON uploads. required to stream large geojson and csv files (> 200 MB) (`-a` unavailable with streaming, use `-p`)

`-h, --help`           output usage information


##### Upload GeoJSON

Upload a GeoJSON file to a new space. XYZ will automatically generate a space ID and display it for you.

    here xyz upload -f /Users/xyz/data.geojson

Upload a GeoJSON file to an existing space.

    here xyz upload SPACE_ID -f /Users/xyz/data.geojson



> #### Hint 
> Use streaming for faster uploads.
> Using streaming via the `upload -s` option will significantly reduce the time required to 
> upload GeoJSON files. The default non-streaming mode is useful for troubleshooting. If your
> features are small, you can also try increasing the chunk size using `-c`.
    
***
    

> #### Note 
> Feature IDs:
> XYZ requires that every feature in a space has a unique id. (This lets you access features in a
> space using the API.) If a GeoJSON feature does not have an ID (a common occurance), XYZ's default
> upload behavior is to create one based on a hash of the feature's property. (If you have duplicate
> records in a dataset at the same location, only the first will be uploaded.)
> If you are certain that your dataset has a well-managed set of unique feature IDs, use `upload -o`
> to have XYZ use them. If you use `-o` to upload a new dataset, and a new feature has the same ID
> as an existing feature, XYZ will consider it an update and overwrite the existing feature with 
> the new feature. Many public datasets often have a "unique" set of feature IDs that are simply
> incremental integers which can lead to feature replacement that you probably didn't want.

##### Upload a CSV file


    here xyz upload -f /Users/xyz/data.csv


XYZ will attempt to choose the columns containing the latitude and longitude fields based on well known names including:

    y, ycoord, ycoordinate, coordy, coordinatey, latitude, lat
    x, xcoord, xcoordinate, coordx, coordinatex, longitude, lon, lng, long, longitud

If your csv uses different names, you can specify the latitude field with `-y` and longitude with `-x`.

    here xyz upload -f /Users/xyz/data.csv -x the_lon -y the_lat

If the csv combines coordinates into a single field, such as
    
    37.7,-122.2
    
or

    (37.7,-122.2)

you can specify the name of that column with `-z`.

    here xyz upload -f /Users/xyz/data.csv -z points

Rows that have `0,0` or `null` values in the designated latitude and longitude columns will be tagged with `null_island`. They will not be displayed on the map, but you can access them via the API (or in geojson.tools) by appending `&tags=null_island` so you can inspect and repair the records.

If the lat/lon columns contain letters or other invalid characters, the features are tagged with `invalid`.


> #### Hint 
> Use streaming for faster uploads.


        Using streaming via the `upload -s` option will significantly reduce the time required to upload CSV files of any size. Standard, non-streaming mode is useful for troubleshooting. 
        

> #### Hint
> Using properties as the feature ID.


    The CLI converts a CSV row into a GeoJSON feature before uploading it. If you want values in a CSV columns to be the GeoJSON feature ID, use `-i columnName.`
        
##### Chunking

You can adjust the "chunk" size when streaming an upload. This controls the number of features that the CLI sends to the API at a time. The default chunk size is 200 features.

###### Large Features

You may see upload errors from the CLI if your features are large, complex geometries. By decreasing the chunk size, you may still be able to upload these large features. Try `-c 100`, or even `-c 10` or `-c 1`. If you continue to see errors you may need to [simplify the geometries](../shapefiles) before uploading them.

###### Small Features

If your features are small, like you might see in a GeoJSON file containing points or a CSV, you will see faster uploads if you increase the chunk size. For example, `-c 1000` will enable the CLI to upload 1000 features at a time. If the features are very simple, `-c 10000` may also be approproate. As long as the chunk size is below the size of the API gateway, this will speed up your upload. The CLI will notify you if there is an upload error. Note that if you need to restart your upload, existing features will not be duplicated.

##### Upload and Stream Large CSV and GeoJSON Files

To upload very large CSV and GeoJSON files to your XYZ space, will will need to use `-s` -- this will stream the file and avoid Node.js memory errors. (It will also be considerably faster than the standard upload method.)

    here xyz upload YOUR_SPACE_ID -f /Users/xyz/big_data.csv -s



> #### Note
> When a file is streamed with `-s` it is not loaded into memory and -a is not available 
> to preview and assign tags. You can specify tags using `-p`.

***
    
> #### Info
> HERE XYZ is a database. Databases trade off storage space for speed, and your data will always 
> take up more space in XYZ than it does in a static file. When a file is uploaded into an XYZ Space,
> features, their properties, and the geometries are broken out into multiple tables, indexed and
> tagged. All of this lets you query your geospatial data on demand, and access it dynamically as
> vector tiles. You can check the size of your XYZ Spaces in your account dashboard or the CLI. 
    
    


##### Upload a Shapefile

```
here xyz upload -f /Users/dhatb/data.shp
```


> #### Note
> Shapefiles cannot be streamed with `-s`.

    
    
Upload shapefile data to a Space.

More tips in the [Working with Shapefiles](../shapefiles) tutorial.
    
    
> #### Hint
> Instead of passing the content as a file with `-f` option you can also pipe the output 
> of  another command directly into the input stream of the HERE Data Hub CLI like
> `cmd | here xyz upload YOUR_SPACE_ID`.
   
   


##### Upload with a Unique ID

```
here xyz upload -f data.csv -i unique_id
```

Upload data to an XYZ space with a feature ID based on the feature's property `unique_id`. 

This feature should be used if your data has well-known and truly unique identifiers that you want to preserve. The XYZ API can [query individual features by feature ID](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeatures), so this can be a valuable method of accessing and updating data.

By default, the CLI will generate a unique feature ID during upload based on a hash of the properties and geometry. 


> #### Note
> Unique IDs are important for XYZ Pro features such as [Virtual Spaces](#virtual-spaces). 
    



> #### Warning
> Many GIS systems will simply assign incrementing integers as feature IDs to every file. 
> These can conflict across files.
    

##### Upload and Assign Tags 

Tags are special properties that can be added to a feature that makes it easy to query them from the XYZ API using the `&tags=` parameter.


> #### Note
> XYZ Tags should be used selectively, ideally using [Rule-Based Tags]().
> Tags are not meant to be a replacement for  [Property Search](#property-search), as you 
> will be duplicating existing data in a record. 


###### Assign Tags Interactively

```
here xyz upload -f file.geojson -a
```

Uploads data and allows users to select tags from a list of feature keynames, with a preview of the first few values. 

###### Assign Tags Using Property Names

```
here xyz upload -f file.geojson -p treatment
```
Uploads data and adds the value of the selected feature property as tag. These tags can be used to filter data when querying the HERE XYZ API. The tags will be stored as `propertyname@value`. This is most effective when the property consists of a limited number of qualitative values.

###### Response

```
treatment@green_paint, treatment@sharrows, treatment@hit_post
```

#### Show Contents of a Space

```
here xyz show YOUR_SPACE_ID
```

Show the objects of a space in table, filter by tags or property values, or open the space in other tools.

##### Options

`-l, --limit <limit>   ` Number of objects to be fetched

`-h, --handle <handle> ` The handle to continue the iteration

`-t, --tags <tags>     ` Filter by tags

`-r, --raw             ` show raw GeoJSON content instead of table

`-s, --search <propfilter> ` search feature properties 

`-p, --prop <prop>     ` property fields to include in table, can be used multiple times

`-w --web         ` display xyz on [http://geojson.tools](http://geojson.tools)

`-v --vector  ` inspect and anayze XYZ spaces using Tangram / [XYZ Space Invader](../space-invader)

##### Filter by Tags

Using `show` on a large space will generate a long table. You can see the raw GeoJSON of the first 5000 features using `-r`. This can also be very long. You may want to direct this output to a file.

    here xyz show spaceID -r > my.geojson

If your space contains a few hundred to a few thousand features, you can open the space in geojson.tools, a data preview tool, using `show -w`. Larger spaces can be previewed in [XYZ Space Invader](../space-invader), a Tangram-based tool from XYZ Labs, using `show -v`.

You can filter tags from XYZ using tags with `-t`:

`here xyz show spaceID -t my_tag` (records with `my_tag` will be printed in the console)
`here xyz show spaceID -w -t my_tag` (records with `my_tag` will be opened in geojson.tools)
`here xyz show spaceID -v -t my_tag` (records with `my_tag` will be opened in XYZ Space Invader)

##### Property Search 

If a property has been indexed by XYZ, you can filter them with `-s` or `--search`. The property name must be prefixed by `p.`:

    here xyz show spaceID -s "p.property_name>value"
    here xyz show spaceID -s "p.name=John,Tom+p.age<50+p.phone='9999999'+p.zipcode=123456" -w
    
- Operators include `=,!=,>,>=,<,<=`
- Search expressions must be enclosed in double quotes, e.g. `"p.property_name>value"`
- Use comma separated values to search multiple values of a property, e.g. `OR`. Use `+` for `AND`.
- Use single quotes to signify a string value, e.g. `"p.property_name>value='100'"` vs `"p.property_name>value=100"`
- To access feature ID, timestamps, or tags, prefix them with `f.`, e.g. `f.id, f.updatedAt, f.tags f.createdAt`
- When accessing Property Search via the API, the URL-safe arguments are `=`, `!=`, `=gt=`, `=gte=`, `=lt=`, `=lte=`.


> #### Note
> Property Search is available in spaces with fewer than 15,000 features by default. 
> For spaces larger than 15,000 features, a limited number will be indexed. To access more, 
> you will need an XYZ Pro license, [learn more about XYZ Pro features here](../xyz_pro).  
    

##### Property Filters

You can use `show -p` or `--prop` to filter the properties that get returned by the API. This is useful when your features have a large number of properties, and you only need to return some of them along with with the geometry.

    here xyz show -p p.property1,p.property2 -w
    

> #### Note 
> Your account needs access to the XYZ Pro Services. 
> [Learn more about XYZ Pro features here](../xyz_pro).


##### Spatial Search

You can use `--spatial` to search for features in an XYZ space that fall within a radius, or a polygon, or along a line. You can specify a point and a radius, a feature in another XYZ space, or a feature in a geojson file.

- `--center`: comma separated `lat,lon` values that specify the center point for the search
- `--radius`: the radius of the search, in meters, from the `--center` point, or a buffer around a geometry specified with `--feature` or `--geometry`
- `--feature`: comma separated `spaceid,featureid` values that specify a reference geometry from another XYZ space -- this will return features from the first space that fall within or along a feature from the second space
- `--geometry`: a single GeoJSON feature in a file to be uploaded for the spatial query

These results are most easily viewable using `show -w`.

#### Delete a Space

```
here xyz delete YOUR_SPACE_ID
```

Delete a Space you have access to.

#### Clear a Space

```
here xyz clear YOUR_SPACE_ID
```

Clear data from your space. You clear the entire space, or clear by tag or feature ID.

##### Options

`-t, --tags [tags]` tags in your Space

`-i, --ids [IDs]` IDs in your Space

`-h --help` output usage information


#### List All Tokens

```
here xyz token
```

Lists all the xyz token you use:

```
id type lat description
--------------- --------- ---------- ------------------------------------------------------------------------------
YOUR_TOKEN_NR_1 PERMANENT 1534451767 xyz-hub=readFeatures,createFeatures,updateFeatures,deleteFeatures,manageSpaces
YOUR_TOKEN_NR_2 PERMANENT 1534516620 xyz-hub=readFeatures
```

#### Get More Information about Your Spaces


> #### Note
> To use this feature, your account needs access to the XYZ Pro Services.
> [Learn more about XYZ Pro features here](../xyz_pro).


You can use the `config` command to get and update information about your spaces.

##### Options

`  --shared <flag>            ` set your space as shared / public (default is false)
`  -s,--schema [schemadef]    ` set schema definition (local filepath / http link) for your space, all future data for this space will be validated for the schema
`  -t,--title [title]         ` set title for the space
`  -d,--message [message]     ` set description for the space
`  -c,--copyright [copyright] ` set copyright text for the space
`  --stats                    ` see detailed space statistics
`  -r, --raw                  ` show raw output
`  -h, --help                 ` output usage information

##### Get Information about a Space

    here xyz config SPACE_ID
    
This will print a table with the title, desciption, and other high-level information about the space.

You can see the raw `json` response from the `/statistics` endpoing using `-r`:

        here xyz config SPACE_ID -r

##### Get a List of Tags and Properties Used in a Space

You can get more details about a space by using the `--stats` option. This will return the number of features, the size of the space, the bbox, geometry types, names and counts of tags, as well as the names of properties (and if they can be accessed via Property Search).

    here xyz config SPACE_ID --stats


> #### Hint
> Use `here xyz analyze` to get a count and list of values of a property in a space. 
> This is best suited for qualitative values. Only the first 500,000 features in a space 
> will be analyzed. 
    

##### Update the Title and Description of a Space

To update the title and/or description of a space:

    here xyz config -t "A meaningful title for a space" -d "additional details about this space that future you will appreciate 6 months from now"

##### Share a Space

You can share a space with other users using the `--shared` option. If they have an XYZ account, they will be able to read from that space using their own tokens (and any data transfer will be charged to their XYZ account).

    here xyz config spaceID --shared true
    
You can disable sharing by passing a `false` parameter:

    here xyz config spaceID --shared false


##### Update, Upload, or Delete a Schema Definition


> #### Note 
> To use this feature, your account needs access to the XYZ Pro Services.
> [Learn more about XYZ Pro features here](../xyz_pro).


A schema validation json file can be configured for a space. The schema definition can be in the form of a web address or a local schema json file. Features that do not match this schema will not be uploaded. 


```
here xyz config YOUR_SPACE_ID -s schema_definition.json
```
To delete a schema from a space:

```
here xyz config YOUR_SPACE_ID -s
```

#### Virtual Spaces


> #### Note 
> To use this feature, your account needs access to the XYZ Pro Services.
> [Learn more about XYZ Pro features here](../xyz_pro).


Virtual Spaces give users access to multiple spaces with one ID. Group lets you bundle your spaces together, and changes get written back to their original spaces. Associate lets you make your own personal edits to a shared space or one with public data, merging the properties of objects with the same feature ID.

    here xyz virtualize|vs -a|-g space1,space2
    
    
##### Group

    here xyz virtualize -g space1,space2,...
    
`group` takes multiple XYZ spaces and presents them via a single XYZ space ID. Duplicates can occur. Any updates will be made to the original spaces.

##### Associate

    here xyz vs -a space1,space2
    
`associate` takes features from `space1` and merges their properties into features with the same feature id in `space2`.

One way of using `virtualize` is to upload CSVs of census data with unique geoID, and merge the statistics on the fly into census geometries where the geoID is the unique ID.

#### Join (Virtual Spaces)

The `join` command simplifies use of virtual spaces when using CSV tables and existing geometries. You can designate a CSV column to be the feature ID, and use the `associate` virtual spaces option to join it with a space with geometries that use the same set of feature IDs. 

    here xyz join space_with_geometries -f data_table.csv -k column_with_id


> #### Note
> `join` creates a space of features with no geometries. You can inspect this space using 
> geojson.tools via `show -w`.

    
    You can update this "csv space" using `here xyz upload spaceID -f new.csv -k id --noGeom` and the next time the virtual space ID is references, the properties will contain the updated values.
    
#### Spatial Search


    
#### GIS Functions


> #### Note 
> To use this feature, your account needs access to the XYZ Pro Services.
> [Learn more about XYZ Pro features here](../xyz_pro).


The CLI has access to a number of convenient geopspatial data functions via the `here xyz gis` command. Some of these functions add properties to the original features, while others create data in a new space. 

- `--area` uses `turf.js` to calculate the area of polygons, and saves this as a set of new properties in each polygon feature. `xyz_area_sqmiles`,`xyz_area_sqkm` are rounded for display convenience, and `xyz_area_sqm` is not rounded.
- `--length` uses `turf.js` to calculate the length of lines in a space, and saves this as a set of new properties in each linestring feature, `xyz_length_miles`,`xyz_length_km` which are rounded for display convenience, and `xyz_length_m` which is not rounded.
- `--centroid` uses `turf.js` to calculate the center of each polygon in a space. By default, these points are written to a new space, but can saved in the existing space using the `--samespace` option. In either case, they all receive a `centroid` tag.
- `--voronoi` uses `d3-delaunay.js` to generate Voronoi polygons from points in an XYZ space. The edges of these polygons are equidistant from two points, and the vertices are equidistant to three points. By default, they are written to a new space, but can saved in the source point space using the `--samespace` option. In either case, they all receive a `voronoi` tag. 
- `--tin` uses `d3-delaunay.js` to generate Delaunay triangles from points in an XYZ space. This process maximizes the minimum angle of all the angles of the triangles created from the source points. By default, they are written to a new space, but can saved in the source point space using the `--samespace` option. In either case, they all receive a `tin` tag. 

#### Hexbins


> #### Note 
> Your account needs access to the XYZ Pro Services.
> [Learn more about XYZ Pro features here](../../xyz_pro).


Hexbins are a data simplification method that makes it easier to visualize large datasets of point features at low zoom levels (continent, country, state/province). A series of hexagon grids are created and the points that fall inside each are counted and written to a new XYZ space, and statistics are calculated across the hexbin grid. 

These hexagons (or their centroids) and their statistics can be quickly displayed in place of the raw data that might overwhelm a renderer. Default colors indicating relative "occupancy" are generated for convenience of display.

 `here xyz hexbin spaceID -z 5-10` create hexbins appropriate for zoom levels 5 through 10

 `here xyz hexbin spaceID -z 8,10,12` create hexbins appropriate for zoom 8,10,12

 `here xyz hexbin spaceID -c 100,1000,100000` create hexbins that are 100 meters, 1km and 10km wide

Hexbins are tagged by zoom level, width, and type, making it easy to extract one set from the hexbin space for display and comparison.

You can learn more about hexbins and how to display them [in this tutorial](../hexbins).

##### Data Contained in XYZ Hexbins

Hexbin features contain various values that can help with analysis and visualization:
- `count`: the number of points in a hexbin 
- `maxCount`: the largest number of points in any hexbin across that particular zoom level or cell width
- `occupancy`: `count/maxCount`, how "full" that hexbin is compared to other across that particular zoom level or cell width
- `color`: an `hsla` color range that correlates to relative occupancy (red = "full", green = "average", blue = "empty
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

##### Hexbin Sum and Average

If a property is qualitative (property values, income, population), in addition to counting points, XYZ Hexbins can add up the value of the properties in each hexbin as well as calculate the average.

    here xyz hexbin spaceID -z 10 -a incidents

```
        "sum": {
          "sum": 4071,
          "maxSum": 5117,
          "average": 8.698717948717949,
          "property_name": "incidents"
        }
```
        
##### Hexbin Subcounts
You can also specify a `subcount` within each hexbin based upon the count of the values of particular property.

    `here xyz hexbin spaceID -z 8-12 -p business_type`
    
This would create a `subcount` object in each hexbin, which would contain the relative count of that property value across the hexbin grid.

```
        "count": 48,
        "maxcount": 400,
        "subcount": {
          "bar": {
            "color": "hsla(181, 100%, 50%,0.51)",
            "count": 3,
            "maxCount": 32,
            "occupancy": 0.09375
          },
          "grocery_store": {
            "color": "hsla(158, 100%, 50%,0.51)",
            "count": 5,
            "maxCount": 24,
            "occupancy": 0.20833333333333334
          },
          "restaurant": {
            "color": "hsla(0, 100%, 50%,0.51)",
            "count": 20,
            "maxCount": 40,
            "occupancy": 1
          }...
```

##### Options

`  -c, --cellsize <cellsize>     ` size of hexgrid cells in meters, comma-separate multiple values

`  -i, --ids                     ` add IDs of features counted within the hexbin as an array inside the property of the hexbin created

`  -p, --groupBy <groupBy>       ` name of the feature property by which hexbin counts will be further grouped. subcounts for unique values will be available as objects in the feature

`  -a, --aggregate <aggregate>   ` name of the feature property used for aggregating sum value of all the features inside hexbin. A sum object will be created, with relative and max sum, and average.

`  -r, --readToken <readToken>   ` token of another user's source space, from which points will be read

`  -w, --writeToken <writeToken> ` token of another user's target space to which hexbins will be written

`  -t, --tags <tags>             ` only make hexbins for features in the source space that match the specific tag(s), comma-separate multiple values

`  -b, --bbox <bbox>             ` only create hexbins for records inside a specified bounding box - minLon,minLat,maxLon,maxLat

`  -l, --latitude <latitude>     ` latitude which will be used for converting cellSize from meters to degrees

`  -z, --zoomLevels <zoomLevels> ` create hexbins optimized for zoom levels -- comma separate multiple values, (-z 8,10,12) or dash for continuous range (-z 10-15)

`  -h, --help                    ` output usage information

You can create hexbins either by width in meters, or use preset widths appropriate to the zoom level.