#Virtual Space Tips & Tricks Virtual Spaces allow multiple spaces to be presented via a single space ID. There are two primary methods, `group` and `associate`. This tutorial focuses on `associate`

The command `here xyz vs -a space1,space2` will take the features in space1 and merge the properties into features with the same feature ID in space2. You can also use the `join` command which uploads a new data table and makes the virtual space in one step.

Virtual spaces are especially convenient when you have a spaces containing different data tables and want to merge it into an existing set of geometries. With virtual spaces, you don't have to replicate the geometries, but instead just reuse the geometry space via `associate`.

### --keys
However, not all datasets are going to have feature IDs that match those in our geometry space. In an ideal world, we would all use well-known unique GEOIDs, but in reality the unique identifier may be a name or abbreviation. 

However, if that identifier is present as a property in the geometry table, we can use Data Hub's property search feature to search for records with the matching property, and then assign the features in that data space the same ID as the geometries we are working with.

In this example, the feature ID in the geometry space is the state's abbrevation, and the data table has census IDs. Instead of using `-i` to designate a property to be used as a feature ID, we use `--keys` and specify the field in the CSV that should be searched for in the geometry space. If it matches, the feature in the csv space will inherit the feature ID of that feature in the geometry space, making for a straightforward `--associate`. 

```here xyz join bN9Unt07  -f http://www.appliedgeographic.com/Data/ST_UNEMP.CSV --string-fields ST --keys ST,GEOID```

### --filter

There may be cases where the property we are searching for is not unique in the geometry space -- a good example of this is county names in the United States. If I have a data set where the unique identifier is the county name, I may get false matches from other states. I can restrict the search using another property in the geometry space, in this case "state".

```here xyz join shMPSR4R -f /Users/joram/Downloads/ga_covid_data-2/countycases.csv --keys county_resident,NAME10 --filter state=GA```

This limits the property search to counties in the state of Georgia.


### alternative geometries

Another advantage of using Virtual Spaces is the ability to merge the data table into different spaces containing geometries with different properties (say a space enriched with demographic data) or resolutions

In the US county geometry space config, there is a pointer to another space that contains the centroids of those counties. We generated these using 

    here xyz gis --centroids
    
If we had a data set by county, we could make a virtual space using the polygons, which might be better at high zooms where the shape and borders of the county is meaningingful, as well as the centroids, which would be more useful for investigating the data at a national level at lower zooms, or using a third property to change the size of the point.


or even ones optimized for dataviz, but with a common set of feature IDs. 


Consider [this hexgrid of US states](https://s3.amazonaws.com/xyz-demo/scenes/xyz_tangram/index.html?space=UZ4LdhAn&token=AKj2021fSByO2JUUUQqx5QA&basemap=none&projection=mercator&demo=0&vizMode=range&buildings=1&pattern=&patternColor=%2384c6f9&points=15&lines=0&outlines=5&places=1&roads=1&clustering=0&quadCountmode=mixed&quadRez=4&hexbins=0&voronoi=0&delaunay=0&water=0&label=date%5B20200622%5D.positiveIncrease&property=date%5B20200622%5D.positiveIncrease&palette=viridis&paletteFlip=false&rangeFilter=1&sort=values&hideOutliers=false&pointSizeProp=&pointSizeRange=%5B4%2C20%5D&propertySearch=%7B%7D#4/40.54/-96.47).
