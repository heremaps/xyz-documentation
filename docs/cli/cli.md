# HERE CLI

In this section we give you a quick overview of the most commonly used commands to interact
with XYZ Spaces from the HERE CLI.

!!! warning "Configuration is required for HERE CLI to work"

    If you have skipped the previous section now is the time to go back and make sure
    HERE CLI is properly configured with your developer identity. In case you are not
    sure, you can run `here configure verify` to check if your credentials are valid.

## Supported Commands

HERE CLI is built to be modular and extensible, so it is entirely possible that when you
use the tool it has already learned a couple of new tricks.

The general structure is that you call the `here` command followed by *command*
which essentially corresponds to the API you want to interact with or get more information
from.

### help

You can always call up help on the HERE CLI either by not providing any parameter at all or
by using the `--help` switch.

```
here --help
```

### configure

As explained earlier, HERE CLI needs to know you to interact with your Spaces. You can use

```
here configure account
```
to configure with the e-mail address and password you use for your HERE account.

You can also run

```
here configure verify
```

to check if the credentials provided can be used to interact with HERE APIs.

### xyz

The `xyz` command is used to interact with XYZ Spaces.

#### List all Spaces

To list all Spaces you have access to (with or without Schema Validation) you can use

```
here xyz list
```

#### Create a new Space

```
here xyz create -t "sample test xyz" -d "sample creation"
```

When you create a new Space, the SpaceID will be generated automatically.

##### Options

`-t <title>` title for space

`-d <desc>` description for space

`-s <schema definition>` schema for space with schema validation

When you have many spaces, you will be glad you added meaningful titles and descriptions.
The schema definition can be in the form of a web address or a schema json.

!!! note "Schema validation is not transactional (yet)"
    The schema validation will be non transactional (Transaction = FALSE) i.e. upload all the objects which passes schema definition and display the list of objects rejected.

#### Upload/Update data to a Space

##### Options

`-f, --file <file>`    geojson file to upload

`-c, --chunk [chunk]`  chunk size (adjusts the number of features uploaded at once)

`-t, --tags [tags]`    tags for the xyz space (used to filter data from the API)

`-x, --lon [lon]`      choose longitude field name, if not well known

`-y, --lat [lat]`     latitude field name, if not well known

`-p, --ptag [ptag]`    property name(s) whose values will to be used to generate tags

`-i, --id [id]`        property name(s) to be used as the feature ID

`-a, --assign`         list a sample of properties, allowing you to assign fields to be selected as tags

`-u, --unique`         option to enforce uniqueness to the id by creating a hash of features to use that as id

`-o, --override`       allow duplicate features to be uploaded even if they share the same feature id

`-s, --stream`        stream large geojson and csv files (> 200 MB) (-a unavailable with streaming, use -p)

`-h, --help`           output usage information

##### Upload geojson

```
here xyz upload YOUR_SPACE_ID -f /Users/xyz/data.geojson
```

Upload geojson data to a Space with name YOUR_SPACE_ID

##### Upload csv

```
here xyz upload YOUR_SPACE_ID -f /Users/xyz/data.csv
```

Upload csv data to a Space with name YOUR_SPACE_ID.

XYZ will attempt to choose the columns containing the latitude and longitude fields based on well known names including:

    y, ycoord, ycoordinate, coordy, coordinatey, latitude, lat
    x, xcoord, xcoordinate, coordx, coordinatex, longitude, lon, lng, long, longitud

If your csv uses different names, you can specify the latitude field with `-y` and longitude with `-x`.

##### Upload and stream large csv and geojson files

You can upload large geojson and csv files to your XYZ space by using `-s` -- this will stream the file and avoid Node memory errors.

    here xyz upload YOUR_SPACE_ID -f /Users/xyz/big_data.csv -s


    here xyz upload YOUR_SPACE_ID -f /Users/xyz/big_data.csv -s

!!! warning "-a is not available with -s, use -p instead"

    When a file is streamed with `-s` it is not loaded into memory and -a is not available to preview and assign tags. You can specify tag using `-p`.

##### Upload a shapefile

```
here xyz upload YOUR_SPACE_ID -f /Users/dhatb/data.shp
```

Upload shapefile data to a Space with name YOUR_SPACE_ID.

More tips in the [Working with Shapefiles](../shapefiles) tutorial.

!!! tip

    Instead of passing the content as a file with `-f` option you can also pipe the output of
    another command directly into the input stream of the HERE CLI like
    `cmd | here xyz upload YOUR_SPACE_ID`

##### Upload with a unique ID

```
here xyz upload YOUR_SPACE_ID -f /Users/dhatb/data.csv -i unique_id
```

Upload data to a Space named YOUR_SPACE_ID with a unique ID of unique_id. This should be used if your data has truly unique identifiers. Note that many GIS systems will assign incrementing integers that conflict across files.

##### Upload and assign tags

```
here xyz upload YOUR_SPACE_ID -f file.geojson -a
```

Uploads data and allows users to select tags from a list of feature keynames, with a preview of the first few values. These tags can be used to filter data when querying the HERE XYZ API.

###### Response

Use the up and down arrows to navigate, and the space bar to select values:

```
1: year: 1996, 2005, 2015
2: buffered: NO, YES, YES
3: treatment: GREEN PAINT, SHARROWS, HIT POSTS
4: globalid: c74ea99ef, 2d88ec21, 8e2ecaa2
5: streetname: FOLSOM ST, 19th AVE, VALENCIA ST
6: class: CLASS III, CLASS I, CLASS IV
7: length: 263.2, 120.9, 708.4
```
Press return to select these values.

###### Response

```
year@1996, 1996, treatment@green_paint, green_paint streetname@folsom_st, folsom_st class@class_iv, class_iv
```

##### Upload with a Property Tag

```
here xyz upload -f file.geojson -p treatment
```
Uploads data and adds the value of the selected feature property as tag. These tags can be used to filter data when querying the HERE XYZ API.

###### Response

```
treatment@green_paint, treatment@sharrows, treatment@hit_post
```

#### Show contents of a Space

```
here xyz show YOUR_SPACE_ID
```

Show the objects of a Space in table.

##### Options

`-l, --limit <limit>   ` Number of objects to be fetched

`-h, --handle <handle> ` The handle to continue the iteration

`-t, --tags <tags>     ` Tags to filter on

`-r, --raw             ` show raw GeoJSON content instead of table

`-p, --prop <prop>     ` property fields to include in table, can be used multiple times

`-w --web [web]        ` display xyz on [http://geojson.tools](http://geojson.tools)

#### Delete a Space

```
here xyz delete YOUR_SPACE_ID
```

Delete a Space you have access to.

#### Clear a Space

```
here xyz clear YOUR_SPACE_ID
```

Clear data from your Space

##### Options

`-t, --tags [tags]` tags in your Space

`-i, --ids [IDs]` IDs in your Space

`-h --help` output usage information

#### Transform files

The system attempts to autodetect the latitude and longitude field name from these matched field names:

* longitude -> "x", "xcoordinate", "coordx", "coordinatex", "longitude", "lon"
* latitude -> "y", "ycoord", "ycoordinate", "coordy", "coordinatey", "latitude", "lat"

##### Transform csv to geojson

```
here transform csv2geo filename.csv
```

##### Transform a shapefile to geojson

```
here transform shp2geo filename.shp
```

##### Options

`-x, --lon [lon]` longitude field name

`-y, --lat [lat]` latitude field name

#### List all tokens

```
here xyz token
```

Lists all the xyz token you use

```
id type lat description
--------------- --------- ---------- ------------------------------------------------------------------------------
YOUR_TOKEN_NR_1 PERMANENT 1534451767 xyz-hub=readFeatures,createFeatures,updateFeatures,deleteFeatures,manageSpaces
YOUR_TOKEN_NR_2 PERMANENT 1534516620 xyz-hub=readFeatures
```

#### Learn more about your spaces

##### Get a list of tags used in a space

```
here xyz analyze spaceID
```

###### Response

```
here xyz describe spaceID
Operation may take a while. Please wait ......
==========================================================
                     Summary for Space zcispwpB
==========================================================
Total 891 features
GeometryType  Count
------------  -----
Point         891

Total unique tag Count : 19
Unique tag list  :["small","type@small","ne_10m_airports","mid","type@mid","mid_and_military","type@mid_and_military","major_and_military","type@major_and_military","military_mid","type@military_mid","military","type@military","major","type@major","military_major","type@military_major","spaceport","type@spaceport"]
TagName                  Count
-----------------------  -----
ne_10m_airports          891
mid                      475
type@mid                 475
type@major               367
major                    367
type@mid_and_military    14
major_and_military       14
type@major_and_military  14
mid_and_military         14
type@military_mid        10
military_mid             10
type@military_major      4
military_major           4
spaceport                3
type@spaceport           3
type@military            2
military                 2
type@small               2
small                    2
```


!!! warning "`describe` only returns the first 500,000 features in a space"

    In order to avoid Node.js memory errors, only the first 500,000 features are described.

##### Get a count of values of a property in a space

```
here xyz analyze spaceID
```

###### Response

Use the arrow keys and select a property by pressing the space bar:

```
here xyz analyze zcispwpB
Operation may take a while. Please wait ......
? Select the properties to analyze
 ◯ 1 : name : Sahnewal , Solapur , Birsa Munda
❯◉ 2 : type : small , mid , mid
 ◯ 3 : abbrev : LUH , SSE , IXR
 ◯ 4 : gps_code : VILD , VASL , VERC
 ◯ 5 : location : terminal , terminal , terminal
 ◯ 6 : iata_code : LUH , SSE , IXR
 ◯ 7 : natlscale : 8 , 8 , 8
```

In this case, the CLI will count and sort the values of the `type` property.

```
PropertyName  Value               Count
------------  ------------------  -----
type          mid                 475
type          major               367
type          mid and military    14
type          major and military  14
type          military mid        10
type          military major      4
type          spaceport           3
type          small               2
type          military            2

Total unique property values in space zcispwpB :

PropertyName  Count
------------  -----
type          9
```

#### Update or Upload Schema

Use

```
here xyz config YOUR_SPACE_ID -s <updated schema definition>
```

to update or upload the schema to an existing space

#### Delete Schema

```
here xyz config YOUR_SPACE_ID -s
```

Deletes the schema from the space
