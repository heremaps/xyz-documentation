# HERE CLI

In this section we give you a quick overview of the most commonly used commands to interact
with Data Hub Spaces from the HERE CLI.

> #### Warning
>
>**Configuration is required for HERE CLI to work**
>
> If you have skipped the previous section now is the time to go back and make sure
    HERE CLI is properly configured with your developer identity. In case you are not
    sure, you can run `here configure verify` to check if your credentials are valid.

## Supported Commands

HERE CLI is built to be modular and extensible, so it is entirely possible that when you
use the tool it has already learned a couple of new tricks.

The general structure is that you call the `here` command followed by *command*
which essentially corresponds to the API you want to interact with or get more information
from.

### Getting help

You can always call up help on the HERE CLI either by not providing any parameter at all or
by using the `--help` switch.

```console
here --help
```

### Log-in to your Data Hub account

As explained earlier, HERE CLI needs to know you to interact with your Data Hub Spaces. You can use

```console
here configure account
```

to configure with the e-mail address and password you use for your HERE account.

You can also run

```console
here configure verify
```

to check if the credentials provided can be used to interact with HERE APIs.

If you have upgraded your HERE account to an [Add-On plan](https://developer.here.com/pricing), you can run

```console
here configure refresh
```

to enable using Data Hub Add-on features. [Learn more](datahub_add-on.md) about Data Hub Add-on features here.

[Click here](add-on.md) to learn how to setup and use Data Hub Add-on features Using HERE CLI.

### [Interact with Data Hub Spaces](command-reference.md#xyz)

The `xyz` command is used to interact with Data Hub Spaces.

You can find the supported subcommands with the `--help` switch:

```console
here xyz --help
```

#### [List Spaces](command-reference.md#list)

To list all Spaces you have access to, you can use

```console
here xyz list
```

##### Options

`-r, --raw` shows the raw space list definition

`--token <token>` an external token to access another user's spaces

Use this option to list another user's spaces using an access token they have shared with you.

> #### Tip
>
> You can use this option with many CLI commands to access another user's spaces. Check for `--token` option in command's help.

`--filter <filter>` a comma separated list of strings to filter spaces by, based on title or description text

Lists only the spaces where the title or description contain the matching string(s).

`-p, --prop <prop>` choose which space property to show when listing spaces: id, title or description.

#### [Create a new Space](command-reference.md#create)

```console
here xyz create -t "sample test xyz" -d "sample creation"
```

When you create a new space, the SpaceID will be automatically generated.

> #### Tip
>
> The `upload` command can also automatically generate a new space ID for you if it is not specified

##### Options

`-t <title>` title for the space

`-d <desc>` description for the space

> #### Tip
>
> When you have many spaces, you will be glad you added meaningful titles and descriptions.

`--token <token>` an external token to create a space in another user's account (must have appropriate admin permissions)

`-s <schema definition>`  set a json schema definition for your space

Applies a schema validation json file to the space to be applied to future uploads. The schema definition can be in the form of a url or a local schema json file. Features that do not match this schema will not be uploaded to the space.

> #### Note
>
> This is an Add-on feature that requires a license. Learn more about [Data Hub Add-on](datahub_add-on.md) features here.

#### [Upload/Update data to a Space](command-reference.md#upload)

The CLI makes it easy to upload geospatial files to a Data Space, and there are a large number of options to enhance, optimize and speed up the upload process.

##### Upload GeoJSON

Upload a GeoJSON file to a new space. Data Hub will automatically generate a space ID and display it for you.

```console
here xyz upload -f /Users/xyz/data.geojson
```

Upload a GeoJSON file to an existing space.

```console
here xyz upload SPACE_ID -f /Users/xyz/data.geojson
```

> #### Tip
>
> **Use streaming for faster uploads**
>
> Streaming with the `upload -s` option will significantly reduce the time required to upload GeoJSON files. The default non-streaming mode is useful for troubleshooting. If your features are small, you can also try increasing the chunk size using `-c`. Chunk size for many point datasets can be safely increased to 1000 or more (the default is 200).

> #### Note
>
> Data Hub requires that every feature in a space has a unique ID. This is to access individual features in a space using the API.
>
> If a GeoJSON feature does not have an ID (a common occurance), Data Hub's default upload behavior is to create one based on a hash of the feature's property &mdash; if you have records with duplicate IDs in a dataset at the same location with the same properties, only one will be uploaded. You can define a new feature ID using more than one property using `-i`.)
>
> If you are certain that your dataset has a well-managed set of unique feature IDs, use `upload -o` to have Data Hub use them. If you use `-o` to upload a new dataset, and a new feature has the same ID as an existing feature, Data Hub will consider it an update and overwrite the existing feature with the new feature. Note that many public datasets often have a "unique" set of feature IDs that are simply incremental integers which can lead to feature replacement that you probably didn't want when you are uploading multiple files to the same space.

##### Upload a CSV file

```console
here xyz upload -f /Users/xyz/data.csv
```

Data Hub will attempt to choose the columns containing the latitude and longitude fields based on well known names including:

> y, ycoord, ycoordinate, coordy, coordinatey, latitude, lat
> x, xcoord, xcoordinate, coordx, coordinatex, longitude, lon, lng, long, longitud

If your csv uses different names, you can specify the latitude field with `-y` and longitude with `-x`. (If you think this is a common field and should be automatically included, [file an issue](https://github.com/heremaps/here-cli/issues) or better yet, [create a pull request](https://github.com/heremaps/here-cli/blob/master/src/transformutil.ts#L39)!)

```console
here xyz upload -f /Users/xyz/data.csv -x the_lon -y the_lat
```

If the csv combines coordinates into a single field, such as

> 37.7,-122.2

or

> (37.7,-122.2)

you can specify the name of that column with `-z`. (Thanks to user ToonvanStrijp for some nice regex.)

```console
here xyz upload -f /Users/xyz/data.csv -z points
```

Rows that have `0,0` or `null` values in the designated latitude and longitude columns will be tagged with `null_island`. They will not be displayed on the map, but you can access them via the API (or in geojson.tools) by appending `&tags=null_island` so you can inspect and repair the records.

If the lat/lon columns contain letters or other invalid characters, the features are tagged with `invalid`.

If you specify `upload --noCoords`, the CLI will upload the CSV rows as features with no geometry. This is useful for working with data tables that you want to dynamically merge with geometries using Virtual Spaces.

> #### Tip
>
> **Use streaming for faster uploads**
>
> Using streaming via the `upload -s` option will significantly reduce the time required to upload CSV files of any size. Standard, non-streaming mode is useful for troubleshooting.

> #### Tip
>
> **Using properties as the feature ID**
>
> The CLI converts a CSV row into a GeoJSON feature before uploading it. If you want values in a CSV column to be the GeoJSON feature ID, use `-i columnName.` Note that you can choose more than one column to create the feature ID.

###### Grouping multiple rows into a single unique feature ID

You can use `--groupby` with `-i` to consolidate multiple rows "belonging to" a unique ID as nested objects within a single feature. Please read the [Group By tutorial](tutorials/group_by.md) for more details.

##### Chunking

You can adjust the "chunk" size when streaming an upload. This controls the number of features that the CLI sends to the API at a time. The default chunk size is 200 features.

###### Large features

You may see upload errors from the CLI if your features are large, complex geometries. By decreasing the chunk size, you may still be able to upload these large features. Try `-c 100`, or even `-c 10` or `-c 1`. If you continue to see errors you may need to [simplify the geometries](tutorials/shapefiles.md) before uploading them.

###### Small features

If your features are small, like you might see in a GeoJSON file containing points or a CSV, you will see faster uploads if you increase the chunk size. For example, `-c 1000` will enable the CLI to upload 1000 features at a time. If the features are very simple, `-c 10000` may also be approproate. As long as the chunk size is below the size of the API gateway, this will speed up your upload. The CLI will notify you if there is an upload error. (You can see more detailed errors using `-e`.)

Note that if you need to restart your upload, existing features will not be duplicated.

##### Upload and stream large CSV and GeoJSON files

To upload very large CSV and GeoJSON files to your Data Hub space, will will need to use `-s` -- this will stream the file and avoid Node.js memory errors. It will also be considerably faster than the standard upload method. (Note that you cannot currently stream a shapefile.)

```console
here xyz upload YOUR_SPACE_ID -f /Users/xyz/big_data.csv -s
```

> #### Note
>
> When a file is streamed with `-s` it is not loaded into memory and -a is not available to preview and assign tags. You can specify tags using `-p`.

> #### Note
>
> HERE Data Hub is a database. Databases trade off storage space for speed, and your data will always take up more storage space in Data Hub than it does in a static file. When a file is uploaded into a Data Hub Space, features, their properties, and the geometries are broken out into multiple tables, indexed and tagged. All of this lets you query your geospatial data on demand, and access it dynamically as vector tiles. You can check the size of your Data Hub Spaces in your account dashboard or the CLI.

##### Upload a shapefile

```console
here xyz upload -f /Users/dhatb/data.shp
```

> #### Note
>
> Shapefiles cannot be streamed with `-s`.

Upload shapefile data to a Space.

> #### Tip
>
> Instead of passing the content as a file with `-f` option you can also pipe the output of
    another command directly into the input stream of the HERE CLI like
    `cmd | here xyz upload YOUR_SPACE_ID` -- this can be useful when piping data from geospatial tools like `mapshaper`.

There are many, many more tips in the [Working with Shapefiles](tutorials/shapefiles.md) tutorial.

##### Upload with a unique ID

```console
here xyz upload -f data.csv -i unique_id
```

Upload data to a Data Hub space with a feature ID based on the feature's property `unique_id`.

This feature should be used if your data has well-known and truly unique identifiers that you want to preserve. The Data Hub API can [query individual features by feature ID](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/getFeatures), so this can be a valuable method of accessing and updating data.

By default, the CLI will generate a unique feature ID during upload based on a hash of the properties and geometry.

> #### Note
>
> Unique IDs are important for Data Hub Add-on features such as [Virtual Spaces](datahub_add-on.md#virtual-spaces).

> #### Warning
>
> Many GIS systems will simply assign incrementing integers as feature IDs to every file. These can conflict across files.

##### Upload and assign tags

Tags are special properties that can be added to a feature that makes it easy to query them from the Data Hub API using the `&tags=` parameter.

> #### Note
>
> Data Hub Tags should be used selectively, ideally using [Rule-Based Tags](add-on.md#rule-based-tags). Tags are not meant to be a replacement for [Property Search](#property-search) as you will be duplicating existing data in a record.

###### Assign tags interactively

```console
here xyz upload -f file.geojson -a
```

Uploads data and allows users to select tags from a list of feature keynames in shapefiles, CSVs and GeoJSON files, with a preview of the first few values.

> #### Note
>
> `-a` does not allow you to stream the upload.

##### Assign tags using property names

```console
here xyz upload -f file.geojson -p treatment
```

Uploads data and adds the value of the selected feature property as tag. These tags can be used to filter data when querying the HERE Data Hub API. The tags will be stored as `propertyname@value`. This is most effective when the property consists of a limited number of qualitative values.

###### Response

```console
treatment@green_paint, treatment@sharrows, treatment@hit_post
```

##### Upload data with timestamp and date properties

If you have timestamp or date properties in your data, the CLI can help you create additional time and date specific properties and time-based tags.

```console
here xyz upload <SPACE_ID> -f <CSV|GEOJSON> --date <propertyname>
```

Convert a valid date/timestamp property into an ISO 8601 date and a unix timestamp and adds them as `datahub_timestamp_propertyname` and `datahub_iso8601_propertyname` properties to the features.

```console
here xyz upload <SPACE_ID> -f <CSV|GEOJSON> --date <propertyname> --dateprops [year,month,week,weekday,year_month,year_week,hour]
```

Extract the date parameter(s) specified following `--dateprops` from the property and upload them as individual properties to the features.

So for an example property in a feature: `"start_time": "2020-04-29T22:00:00+05:30"`, you will get the following properties added to the feature upon upload.

|  --dateprops    |         property             | value
| --------------  | ---------------------------- |---------------
|      year       | date_start_time_year         | 2020
|     month       | date_start_time_month        | April
|     week        | date_start_time_week         | 18
|    weekday      | date_start_time_weekday      | Wednesday
|   year_month    | date_start_time_year_month   | 2020-04
|   year_week     | date_start_time_year_week    | 2020-18
|      hour       | date_start_time_hour         | 22

```console
here xyz upload <SPACE_ID> -f <CSV|GEOJSON> --date <propertyname> --datetag [year,month,week,weekday,year_month,year_week,hour]
```

Along similar lines, `--datetag` will let you specify which date parameter tags need to be created out of a date property for the features.

|  --datetag      |         tag                  |
| --------------  | ----------------------------  
|      year       | date_start_time_year@2020    |
|     month       | date_start_time_month@april  |
|     week        | date_start_time_week@18         |
|    weekday      | date_start_time_weekday@wednesday    |
|   year_month    | date_start_time_year_month@2020-04   |
|   year_week     | date_start_time_year_week@2020-18    |
|      hour       | date_start_time_hour@22        |

##### Upload history of a space

The CLI saves the 3 most recent upload commands for a space within the space metadata. This allows you to re-use one of them later with the `--history` option. You can specify a command index which can range from 0(newest) to 2 (oldest), or you can use the `--history` option without an index, and you will be provided with an interactive list of historical upload commands.

```console
here xyz upload <SPACE_ID> --history [0-2|blank]
```

You can also mark one of the history commands as a "favorite" so that you can later re-execute it.

```console
here xyz upload <SPACE_ID> --history save
```

To execute the favorite upload command:

```console
here xyz upload <SPACE_ID> --history fav
```

This is a convenient way to save upload commands with particularly complex options, especially when you revisit a space a few months after you last worked with it.

Note that this will immediately upload the command.

You can clear a space's `history` using `clear`

##### Upload multiple files with batch upload

You can upload a directory full of geospatial files to a space in one command using `--batch`. This is convenient way to rejoin datasets that have been broken into multiple files, or to add regional files to one space in order to build a national dataset.

You must specify the directory with `-f` and the filetype after `--batch`.

```console
here xyz upload <SPACE_ID> --batch [geojson|csv|shp|gpx] -f <PATH_TO_FOLDER>
```

While uploading shapefiles, `--batch` will inspect one level of sub-directories within that specified directory to look for `.shp` and all other relevant files from when uncompressing a zipped shapefile.

##### Options

`-f, --file <file>`   GeoJSON, GeoJSONL, CSV or Shapefile to upload

`-c, --chunk [chunk]` chunk size (adjusts the number of features uploaded at once)

`-t, --tags [tags]`   tags for the Data Hub space (used to filter data from the API)

`--token <token>`    a external token to upload data to another user's space

`-x, --lon [lon]`     choose longitude CSV field name, if not well known

`-y, --lat [lat]`     latitude CSV field name, if not well known

`-z, --point [point]`  points field name, e.g. `(lat,lon)`

`--lonlat`              parse a --point/-z csv field as `(lon,lat)` instead of `(lat,lon)`

`-p, --ptag [ptag]`    property name(s) whose values will be used to generate tags

`-i, --id [id]`        property name(s) to be used as the feature ID

`-a, --assign`         lists a sample of properties, allowing you to assign fields to be selected as tags and Feature IDs

`-o, --override`       override default property hash feature ID generation and use existing GeoJSON feature IDs

`-d, --delimiter [,]`  an alternate delimiter used in CSV (default: ",")

`-q, --quote ["]`       quote used in CSV (default: "\"")

`-e, --errors`         print data upload errors

`-s, --stream`        speed up CSV and GeoJSON uploads. Required to stream large geojson and csv files (> 200 MB) (`-a` unavailable with streaming, use `-p`)

`--string-fields <stringFields>`  property name(s) of CSV string fields *not* to be automatically converted into numbers or booleans (e.g. number-like census geoids, postal codes with leading zeros)

`--groupby <groupby>`             consolidate multiple rows of a CSV into a single feature based on a unique ID designated with -i; values of each row within the selected column will become top level properties within the consolidated feature

`--date <date>`                   date-related property name(s) of a feature to be normalized as an ISO 8601 datestring (`xyz_iso8601_[propertyname]`), and unix timestamp (`xyz_timestamp_[propertyname]`)

`--datetag [datetagString]`       comma separated list of granular date tags to be added via --date. possible options - year, month, week, weekday, year_month, year_week

`--dateprops [datepropsString]`   comma separated list of granular date properties to be added via --date. possible options - year, month, week, weekday, year_month, year_week

`--noCoords`                      upload CSV files with no coordinates, generates null geometry (best used with -i and virtual spaces)

`--history [history]`             repeat commands previously used to upload data to a space; save and recall a specific command using "--history save" and "--history fav"

`--batch [batch]`                 select type of files to be uploaded in batch (select directory with -f)

`-h, --help`           output usage information

#### [Show contents of a space](command-reference.md#show)

```console
here xyz show YOUR_SPACE_ID
```

Show the objects of a space in table, filter by tags or property values, or open the space in other visualization tools.

Using `show` on a large space will generate a long table. You can see the raw GeoJSON of the first 5000 features using `-r`. This can also be very long. You may want to direct this output to a file, or pipe to `more`.

```console
here xyz show spaceID -r > my.geojson
```

If your space contains a few hundred to a few thousand features, you can open the space in geojson.tools, a data preview tool, using `show -w`.  
Larger spaces can be previewed in [Space Invader](space-invader/index.md), a Tangram-based tool from Data Hub Labs, using `show -v`, and features like H3 hexbin and quadbin clustering can be used to visualize even larger spaces.

##### Filter by Tags

You can filter tags from Data Hub using tags with `-t`:

`here xyz show spaceID -t my_tag` (records with `my_tag` will be printed in the console)
`here xyz show spaceID -w -t my_tag` (records with `my_tag` will be opened in geojson.tools)
`here xyz show spaceID -v -t my_tag` (records with `my_tag` will be opened in Data Hub Space Invader)

##### Property Search

If a property has been indexed by Data Hub, you can filter them with `-s` or `--search`. The property name must be prefixed by `p.`:

```console
here xyz show spaceID -s "p.property_name>value"

here xyz show spaceID -s "p.name=John,Tom+p.age<50+p.phone='9999999'+p.zipcode=123456" -w
```

- Operators include `=`,`!=`,`>`,`>=`,`<`,`<=`
- Search expressions must be enclosed in double quotes, e.g. `"p.property_name>value"`
- Use comma separated values to search multiple values of a property, e.g. `OR`. Use `+` for `AND`.
- Use single quotes to signify a string value, e.g. `"p.property_name>value='100'"` vs `"p.property_name>value=100"`
- To access feature ID, timestamps, or tags, prefix them with `f.`, e.g. `f.id, f.updatedAt, f.tags f.createdAt`
- When accessing Property Search via the API, the URL-safe arguments are `=`, `!=`, `=gt=`, `=gte=`, `=lt=`, `=lte=`.

> #### Note
>
> Property Search is available in spaces with fewer than 15,000 features by default. For spaces larger than 15,000 features, a limited number will be indexed. To access more, you'll need a Data Hub Add-on license, [learn more about Data Hub Add-on features here](datahub_add-on.md).

##### Property Filters

You can use `show -p` or `--prop` to filter the properties that are returned by the Data Hub API. This is useful when your features have a large number of properties, and you only need to return some of them along with with the geometry.

```console
here xyz show -p p.property1,p.property2 -w
```

> #### Note
>
> This feature requires access to  Data Hub Add-on Services. Learn more about [Data Hub Add-on](datahub_add-on.md) features here.

##### Spatial Search

You can use `--spatial` to search for features in a Data Hub space that fall within the radius from a point, or within a polygon, or along a line.

You can specify a point and a radius, or a feature in another Data Hub space, or a GeoJSON file containing a feature.

- `--center`: comma separated `lat,lon` values that specify the center point for the search
- `--radius`: the radius of the search, in meters, from the `--center` point, or a buffer around a geometry specified with `--feature` or `--geometry`
- `--feature`: comma separated `spaceid,featureid` values that specify a reference geometry from another Data Hub space -- this will return features from the first space that fall within or along a feature from the second space
- `--geometry`: a single GeoJSON feature in a file to be uploaded for the spatial query

These results are most easily viewable using `show -w`.

##### Options

`-l, --limit <limit>` Number of objects to be fetched

`-o, --offset <offset>` The offset / handle to continue the iteration

`-t, --tags <tags>` Tags to filter on

`-r, --raw` show raw Data Hub space content

`--all` iterate over entire Data Hub space to get entire data of space, output will be shown on the console in geojson format

`--geojsonl` to print output of --all in geojsonl format

`-c, --chunk [chunk]` chunk size to use in --all option, default 5000

`--token <token>` a external token to access another user's space

`-p, --prop <prop>` selection of properties, use p.\<FEATUREPROP\> or f.<id/updatedAt/tags/createdAt>

`-w, --web` display Data Hub space on [http://geojson.tools](http://geojson.tools)

`-v, --vector` inspect and analyze using [Data Hub Space Invader](space-invader/index.md) and tangram.js

`-s, --search <propfilter>` search expression in "double quotes", use single quote to signify string value,  use p.\<FEATUREPROP\> or f.\<id/updatedAt/tags/createdAt\> (Use
                             '+' for AND , Operators : >,<,<=,<=,=,!=) (use comma separated values to search multiple values of a property) {e.g.
                             "p.name=John,Tom+p.age<50+p.phone='9999999'+p.zipcode=123456"}

`--spatial` indicate to make spatial search on the space

`--radius <radius>` indicate to make radius spatial search or to thicken input geometry (in meters)

`--center <center>` comma separated lon,lat values to specify the center point for radius search

`--feature <feature>` comma separated spaceid,featureid values to specify reference geometry (taken from feature) for spatial query

`--geometry <geometry>` geometry file to upload for spatial query (single feature in geojson file)

`-h, --help` display help for command

#### [Delete a Space](command-reference.md#delete)

```console
here xyz delete YOUR_SPACE_ID
```

Delete a space you have access to. By default you will be shown information about the space for context, and a confirmation prompt.

##### Options

`--force` skip the confirmation prompt

`--token <token>` a external token to delete another user's space

`-h, --help` display help for command

#### [Clear a Space](command-reference.md#clear)

```console
here xyz clear YOUR_SPACE_ID
```

Clear data from your space. You clear the entire space, or clear by tag or feature ID. By default you will be shown information about the space for context, and a confirmation prompt.

##### Options

`-t, --tags <tags>` tags for the Data Hub space

`-i, --ids <ids>` ids for the Data Hub space

`--token <token>` a external token to clear another user's space data

`--force` skip the confirmation prompt

`-h, --help` display help for command

#### [List all tokens](command-reference.md#token)

```console
here xyz token
```

Lists all the Data Hub tokens you have availble:

```console
id             | type    | lat      | description
--------------- --------- ---------- ------------------------------------------------------------------------------
YOUR_TOKEN_NR_1 PERMANENT 1534451767 xyz-hub=readFeatures,createFeatures,updateFeatures,deleteFeatures,manageSpaces
YOUR_TOKEN_NR_2 PERMANENT 1534516620 xyz-hub=readFeatures
```

##### Options

`--console` opens the web console for Data Hub <https://xyz.api.here.com/console>

`-h, --help` display help for command

#### [Get or update more information about your spaces](command-reference.md#config)

You can use the `config` command to get and update information about your spaces.

##### Get information about a space

```console
here xyz config SPACE_ID
```

This will print a formatted table with the title, desciption, and other high-level information about the space.

You can see the raw `json` response from the `/statistics` endpoing using `-r`:

```console
here xyz config SPACE_ID -r
```

##### Get a list of tags and properties used in a space

You can get more details about a space by using the `--stats` option. This will return the number of features, the size of the space, the bbox, geometry types, names and counts of tags, as well as the names of properties (and if they can be accessed via Property Search).

```console
here xyz config SPACE_ID --stats
```

> #### Tip
>
> Use `here xyz analyze` to get a count and list of values of a property in a space. Note that this is a client-side operation and is best suited for qualitative values. Only the first 500,000 features in a space will be analyzed.

##### Update the title and description of a space

To update the title and/or description of a space:

```console
here xyz config -t "A meaningful title for a space" -d "additional details about this space that future you will appreciate 6 months from now"
```

##### Share a space

You can share a space with other users using the `--shared` option. If they have a Data Hub account, they will be able to read from that space using their own tokens (and any data transfer will be charged to their Data Hub account).

```console
here xyz config spaceID --shared true
```

You can disable sharing by passing a `false` parameter:

```console
here xyz config spaceID --shared false
```

Note that any Data Hub user will be able to read a space you have shared, and will be able to view a list all shared spaces.

If you want to selectively share a space, you should generate a token for just that space using the [Data Hub Console](https://xyz.api.here.com/console).

##### Basic Options

`--shared <flag>` set your space as shared / public (default is false)

`-t,--title [title]` set title for the space

`-d,--message [message]` set description for the space

`-c,--copyright [copyright]` set copyright text for the space

`--cacheTTL <cacheTTL>` set cacheTTL value for the space with valid number

`--stats` see detailed space statistics

`--token <token>` a external token to access another user's space config and stats information

`-r, --raw` show raw json output

`--console` opens web console for Data Hub

`-h, --help` display help for command

##### [Add-on Options](datahub_add-on.md)

> #### Note
>
> To use these features, your account needs access to the Data Hub Add-on Services. Learn more about [Data Hub Add-on](datahub_add-on.md) features here.

`-s,--schema [schemadef]` view or set schema definition (local filepath / http link) for your space, applicable on future data, use with add/delete/update

`--searchable` view or configure searchable properties of an Data Hub space, use with add/delete/update

`--tagrules` add, remove, view the conditional rules to tag your features automatically, use with add/delete/update -- at present all tag rules will be
                               applied synchronously before features are stored ( mode : sync )

`--delete` use with schema/searchable/tagrules options to remove the respective configurations

`--add` use with schema/searchable/tagrules options to add/set the respective configurations

`--update` use with tagrules options to update the respective configurations

`--view` use with schema/searchable/tagrules options to view the respective configurations

`--activitylog` configure activity logs for your space interactively

### Transform csv, shp and gpx to geojson

The `here transform` command converts CSVs, shapefiles, and GPX files to GeoJSON. Note this will generate raw GeoJSON and not save it to a space. The `upload` command uses `transform`.

### Geocode locations

The `here geocode` command takes an address string and uses the HERE Geocoder to return a GeoJSON feature containing the coordinates. This returns raw GeoJSON and does not save it to a space.
