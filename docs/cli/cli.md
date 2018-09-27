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
here configure set
```

to configure the AppId and AppCode. It will immediately try to verify these credentials.
Alternatively you can run

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

To list all Spaces you have access to you can use

```
here xyz list
```

This command will list all the Spaces for which the user is authorized.

#### Create a new Space

```
here xyz create -t "sample test xyz" -d "sample creation"
```

Create a new Space, the SpaceId will be generated automatically.

##### Options

`-t <title>` title for space

`-d <desc>` description for space

#### Upload/Update data to a Space

##### Options

`-a` interactively assign tags in the form 'key@value'

`-c, --chunk [chunk]`  chunk size for large uploads

`-f, --file <path>` path to file name

`-i <unique ID>` property containing unique values

`-p, --ptag [ptag]` property key and indexed tag

`-t, --tags [tags]`    tags for the features

`-x, --lon [lon]` longitude field name

`-y, --lat [lat]` latitude field name

`-z, --alt [alt]` altitude field name

##### Upload geojson

```
here xyz upload YOUR_SPACE_ID -f /Users/dhatb/data.geojson
```

Upload geojson data to a Space with name YOUR_SPACE_ID

##### Upload csv

```
here xyz upload YOUR_SPACE_ID -f /Users/dhatb/data.csv
```

Upload csv data to a Space with name YOUR_SPACE_ID

##### Upload shapefile

```
here xyz upload YOUR_SPACE_ID -f /Users/dhatb/data.shp
```

Upload shapefile data to a Space with name YOUR_SPACE_ID

!!! tip

    Instead of passing the content as a file with `-f` option you can also pipe the output of
    another command directly into the input stream of the HERE CLI like
    `cmd | here xyz upload YOUR_SPACE_ID`

##### Upload with unique ID

```
here xyz upload YOUR_SPACE_ID -f /Users/dhatb/data.csv -i unique_id
```

Upload data to a Space named YOUR_SPACE_ID with a unique ID of unique_id.

##### Upload with assign

```
here xyz upload YOUR_SPACE_ID -f file.geojson -a
```

Uploads data and provides a list of keynames and previews the first few values.

###### Response

The resulting values are for example:

```
1: year: 1996, 2005, 2015
2: buffered: NO, YES, YES
3: treatment: GREEN PAINT, SHARROWS, HIT POSTS
4: globalid: c74ea99ef, 2d88ec21, 8e2ecaa2
5: streetname: FOLSOM ST, 19th AVE, VALENCIA ST
6: class: CLASS III, CLASS I, CLASS IV
7: length: 263.2, 120.9, 708.4

type numbers of keys (e.g., 1,3,5,6) to convert as key@value tags:
```
You type
```
>1,3,5,6
```

###### Response

```
year@1996, treatment@green_paint, streetname@folsom_st, class@class_iv
```

##### Upload with a Property Tag

```
here xyz upload -f file.geojson -p treatment
```
Uploads data and adds an existing property as tag for which the values are returned as the values of the tag

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

`-w --web [web]        ` display xyz on http://geojson.tools

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
