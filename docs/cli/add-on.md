# HERE CLI Add-on

In this section we give you a quick overview of the most commonly used advanced commands to interact
with XYZ Spaces from the HERE CLI.

## 1. Schema Defination:

A schema validation json file can be configured for a space. The schema definition can be in the form of a web address or a local schema json file. Features that do not match this schema will not be uploaded. User can use local filepath / hyper link to set or view the schema defination.

!!! Note

    User can apply schema validation while creating space using **create** command  and using
    **config** command for existing space. User can use --schema or -s option to apply schema.

### a. Apply schema validation to existing space using filepath [Using Config Command ]

check available Config options using --help or -h command:

```
here xyz config --help  OR  here xyz config -h
```

a.1 Add/Upload schema : 
```
here xyz config YOUR_SPACE_ID --add --schema filepath/schema_definition.json 
```
e.g here xyz config  kqifmFel --add --schema /Users/xyz/schema_defination.json   



a.2 View schema :

```
here xyz config YOUR_SPACE_ID --schema --view 
```
e.g here xyz config  kqifmFel --schema --view

output: 

{
   "definitions": {},
   "$schema": "http://json-schema.org/draft-07/schema#",
   "$id": "http://example.com/root.json",
   "type": "object",
   "title": "The Root Schema",
   "required": [
      "geometry",
      "type",
      "properties"
   ], like this ....


!!! Note

    User needs to upload the data to the same space to check the schema validation is working
    or not.Applied schema validation will not work on existing data.

a.3 Delete schema : 
```
here xyz config YOUR_SPACE_ID -s --delete
```
e.g here xyz config  kqifmFel -s --delete


### b. Apply schema validation to new space using url [Using Create Command ]

check available Create options using --help or -h command:

```
here xyz create --help  OR  here xyz create -h
```

b.1 Add/Upload schema : 
 
```
here xyz create -s website/schema_defination.json
```

e.g here xyz create -s https://xyz.api.here.com/hub/schemas/ZUHvCMys/5b15d45ebfe242cdc1ec78bdd3657e27370cd65da7b5dd202a219bda4d59d22a/1.json


output : XYZ space '66hCJ8uY' created successfully

b.2 View schema :

```
here xyz config YOUR_SPACE_ID --schema --view 
```
e.g here xyz config 66hCJ8uY --schema --view


output: 
{
   "definitions": {},
   "$schema": "http://json-schema.org/draft-07/schema#",
   "$id": "http://example.com/root.json",
   "type": "object",
   "title": "The Root Schema",
   "required": [
      "geometry",
      "type",
      "properties"
   ],
   "properties": {
      "geometry": {
         "$id": "#/properties/geometry",
         "type": "object",
         "title": "The Geometry Schema",
         "required": [
            "type",  like this ...

## 2. Tagrules

!!! Note

    Tagrule is not applicable on existing data. When user apply tagrules they have to upload the data again to check the tagrule details.

### 2.1 Add tagrule

```
here xyz config –tagrules <spaceId> --add
```
><div style="color:black ; background-color:silver; font-style:italic;font-size:10pt">

>e.g </b> here xyz config --tagrules HJtXzHWi --add

>Starting to add a new synchronous rule to automatically tag features..

><b style='color:green'> ? </b>  **Enter a tag name you would like to assign :** <b style='color:Teal'>  name </b>

>Please enter condition(s) for the auto tagging your features with  `name` e.g. "f.id == 123 || (p.country=='USA' & p.count<=100)" 

><b style='color:green'> ? </b> **condition :**   <b style='color:Teal'> f.id==123 </b>


### 2.2 View tagrules

```
here xyz config –tagrules <spaceId> or here xyz config –tagrules <spaceId> --view
```

e.g here xyz config --tagrules HJtXzHWi or here xyz config --tagrules HJtXzHWi --view

output:

| Tag_name       | Mode         | Auto_tag_condition     |
| :------------- | :----------: | -----------:           |
|  Name          | Sync        |   f.id==123              |

### 2.3 Delete tagrule [user can delete one or all tagrules using delete command]:

```
here xyz config –tagrules <spaceId> --delete
```
e.g here xyz config --tagrules HJtXzHWi –delete


### 2.4 Update tagrule [User can update tagrule name and conditions using update command] :

```
here xyz config –tagrules <spaceId> --update
```
<div style="color: #ffffff; background-color:silver; font-style:italic;font-size:10pt">

>e.g here xyz config --tagrules HJtXzHWi --update

><b style='color:green'>?</b> **Select tag rule to be updated (Use arrow keys)**

><b style='color:Teal'> > city , rule : p.city=Amsterdam , mode : sync </b>
 
 >Select tagrule using arrow and click enter then 

 ><b style='color:green'>?</b> **Select tag rule to be updated**<b style='color:Teal'> city , rule : p.city=Amsterdam , mode : sync </b>

 ><b style='color:green'>?</b> **Press ENTER to keep existing tag name OR type new tag name** (city)

>Entered New Name:

>? Press ENTER to keep existing tag name OR type new tag name **CityName**

>Press ENTER OR type condition(s) for this tag rule. e.g. "f.id == 123 || (p.country=='USA' & p.count<=100)"

><b style='color:green'>?</b> **condition :**  (p.city=Amsterdam) changed to

><b style='color:green'>?</b> **condition :**  <b style='color:Teal'>p.cityname=Mumbai</b>
</p>
</div>

### 2.5 View updated tagrules

```
here xyz config –tagrules <spaceId> or here xyz config –tagrules <spaceId> --view
```
e.g here xyz config --tagrules HJtXzHWi or here xyz config --tagrules HJtXzHWi --view

output:

| Tag_name       | Mode         | Auto_tag_condition     |
| :------------- | :----------: | -----------:           |
|  CityName          | Sync        |   p.cityname=Mumbai          |


## 3. Searchable

### 3.1 Add Searchable

```
here xyz config <spaceId> --searchable --add 
```
e.g here xyz config fgtdc6tz --searchable --add 

<b style='color:green'>?</b> **Enter the property name to make searchable (create index on ) :** address


### 3.2 View Searchable

```
here xyz config <spaceId> --searchable --view 
```

e.g here xyz config fgtdc6tz --searchable –view

output:

| PropertyName       | Mode         | Searchable     |
| :-------------     | :----------: | -----------:   |
|  city              | Auto         |   true         |
|  address           | Manually     |   true         |


### 3.3 Delete Searchable[User can delete one or all searchable properties using delete command]

```
here xyz config <spaceId> --searchable --delete 
```

e.g here xyz config fgtdc6tz --searchable --delete


## 4. Activitylog

### Check or enable activitylog

```
here xyz config --activitylog <spaceId>
```

><div style="color:black ; background-color:silver; font-style:italic;font-size:10pt">

>e.g here xyz config --activitylog jsopziJd

>activity log for this space is not enabled.

><b style='color:green'>?</b> **Select action for activity log** (Use arrow keys) 

><b style='color:Teal'>enable activity log for this space </b>

>cancel operation 

><b style='color:green'>?</b> **Select action for activity log** <b style='color:Teal'>enable activity log for this space</b>

><b style='color:green'>?</b> **Select storage mode for activity log** (Use arrow keys)

><b style='color:Teal'> > full - store whole object on change</b>

>diff - store only the changed properties

><b style='color:green'>?</b> **Select storage mode for activity log**  <b style='color:Teal'>full - store whole object on change </b>

><b style='color:green'>?</b> **Select state (number of change history to be kept) for activity log** (Use arrow keys)

> 1

><b style='color:Teal'> >2 </b>

> 3

> 4

> 5

><b style='color:green'>?</b> **Select state (number of change history to be kept) for activity log** 

><b style='color:Teal'> 2 </b>

>activity log configuration updated successfully, it may take a few seconds to take effect and reflect.



## 5. Virtual Space

Virtual Spaces give users access to multiple spaces with one ID. Group lets you bundle your spaces together, and changes get written back to their original spaces. Associate lets you make your own personal edits to a shared space or one with public data, merging the properties of objects with the same feature ID.

```
here xyz virtualize|vs -a|-g space1,space2
``` 

##### Options

`  -t,--title [title]         ` Title for virtual XYZ space

`  -d,--message [message]     ` set description for the space

`  -g, --group [spaceids]     ` Group the spaces (all objects of each space will be part of the  
   response) - enter comma separated space ids

` -a, --associate [spaceids]  ` Associate the spaces. Features with same id will be merged into one feature. Enter comma separated space ids [space1,space2] -- space1 properties will be merged into space2 features.

`  -h, --help                 ` output usage information

    
### 5.1 Group

```
here xyz virtualize -g space1,space2,...
```

`group` takes multiple XYZ spaces and presents them via a single XYZ space ID. Duplicates can occur. Any updates will be made to the original spaces.

### 5.2 Associate
```
here xyz vs -a space1,space2
```

`associate` takes features from `space1` and merges their properties into features with the same feature id in `space2`.

One way of using `virtualize` is to upload CSVs of census data with unique geoID, and merge the statistics on the fly into census geometries where the geoID is the unique ID.

## 6. Join (Virtual Spaces)

The `join` command simplifies use of virtual spaces when using CSV tables and existing geometries. You can designate a CSV column to be the feature ID, and use the `associate` virtual spaces option to join it with a space with geometries that use the same set of feature IDs. 

```
here xyz join space_with_geometries -f data_table.csv -k column_with_id
```

##### Options

`-f, --file <file>`   csv to be uploaded and associated

`-k, --keyField <keyField>`  field in csv file to become feature id

`-x, --lon [lon]`                 longitude field name
  
`-y, --lat [lat]`                 latitude field name
  
`-z, --point [point]`             points field name with coordinates like (Latitude,Longitude) e.g.(37.7,-122.4)

`--lonlat`                         parse a —point/-z csv field as (lon,lat) instead of (lat,lon)

`-d, --delimiter [,]`             alternate delimiter used in csv (default: ",")

`-q, --quote ["]`                 quote used in csv (default: "\"")

`--token <token>`               a external token to create another user's spaces

`-s, --stream`                    streaming data for faster uploads and large csv support

`--string-fields <stringFields>`  property name(s) of CSV string fields *not* to be automatically converted into numbers or booleans (e.g. number-like census geoids, postal codes with leading zeros)

`-h, --help`                      display help for command

!!! note 

    `join` creates a space of features with no geometries. You can inspect this space using geojson.tools via `show -w`
    
    You can update this "csv space" using `here xyz upload spaceID -f new.csv -k id --noGeom` and the next time the virtual space ID is references, the properties will contain the updated values.

## 7. GIS

The CLI has access to a number of convenient geopspatial data functions via the `here xyz gis` command. Some of these functions add properties to the original features, while others create data in a new space. 

##### Options

`--centroid`             calculates centroids of Line and Polygon features and uploads in  
                         a different space

`--length`               calculates length of LineString features

`--area`                 calculates area of Polygon features

`--voronoi`              calculates Voronoi Polygons of point features and uploads in      
                         different space

`--tin`                  calculates Delaunay Polygons of point features and uploads in     
                         different space

`--property <property>`  populates Delaunay polygons' properties based on the specified    
                         feature property

`-c, --chunk [chunk]`    chunk size, default 20 -- default for polygons, increase for      
                         faster point feature uploads

`-t, --tags <tags>`      source space tags to filter on

`--samespace`            option to upload centroids/voronoi/tin to same space, use tags to 
                         filter

`-h, --help`             display help for command




- `--area` uses `turf.js` to calculate the area of polygons, and saves this as a set of new properties in each polygon feature. `xyz_area_sqmiles`,`xyz_area_sqkm` are rounded for display convenience, and `xyz_area_sqm` is not rounded.
- `--length` uses `turf.js` to calculate the length of lines in a space, and saves this as a set of new properties in each linestring feature, `xyz_length_miles`,`xyz_length_km` which are rounded for display convenience, and `xyz_length_m` which is not rounded.
- `--centroid` uses `turf.js` to calculate the center of each polygon in a space. By default, these points are written to a new space, but can saved in the existing space using the `--samespace` option. In either case, they all receive a `centroid` tag.
- `--voronoi` uses `d3-delaunay.js` to generate Voronoi polygons from points in an XYZ space. The edges of these polygons are equidistant from two points, and the vertices are equidistant to three points. By default, they are written to a new space, but can saved in the source point space using the `--samespace` option. In either case, they all receive a `voronoi` tag. 
- `--tin` uses `d3-delaunay.js` to generate Delaunay triangles from points in an XYZ space. This process maximizes the minimum angle of all the angles of the triangles created from the source points. By default, they are written to a new space, but can saved in the source point space using the `--samespace` option. In either case, they all receive a `tin` tag. 


## 8. Hexbin

Hexbins are a data simplification method that makes it easier to visualize large datasets of point features at low zoom levels (continent, country, state/province). A series of hexagon grids are created and the points that fall inside each are counted and written to a new XYZ space, and statistics are calculated across the hexbin grid. 

##### Options

`-c, --cellsize <cellsize>`      size of hexgrid cells in meters, comma-separate multiple  
                                 values

`-i, --ids`                      add IDs of features counted within the hexbin as an array 
                                 in the hexbin's feature property

`-p, --groupBy <groupBy>`        name of the feature property by which hexbin counts will  
                                 be further grouped

`-a, --aggregate <aggregate>`    name of the feature property used for aggregating sum     
                                 value of all the features inside a hexbin

`-r, --readToken <readToken>`    token of another user's source space, from which points   
                                 will be read
 
`-w, --writeToken <writeToken>`  token of another user's target space to which hexbins     
                                 will be written
 
`-t, --tags <tags>`              only make hexbins for features in the source space that   
                                 match the specific tag(s), comma-separate multiple values 

`-b, --bbox [bbox]`              only create hexbins for records inside the bounding box   
                                 specified either by individual coordinates provided       
                                 interactively or as minLon,minLat,maxLon,maxLat (use “\ “ 
                                 to escape a bbox with negative coordinate(s))

`-l, --latitude <latitude>`      latitude which will be used for converting cellSize from  
                                 meters to degrees

`-z, --zoomLevels <zoomLevels>`  hexbins optimized for zoom levels (1-18) - comma separate 
                                 multiple values(-z 8,10,12) or dash for continuous        
                                 range(-z 10-15)
  
`-h, --help`                     display help for command


