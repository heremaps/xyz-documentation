# Group By in CSV Files

CSV is a flat file format, but the rows often contain hierarchical data associated with a particular region or place. A few examples include election results, census statistics, or time-series data. If more than one row of your CSV contains an ID for the same place, you can use `upload --groupby` to nest the values of a column as unique properties within a single feature. You can then use Virtual Spaces to join the data to existing geometries on the fly.

- `--groupby columnName` consolidates multiple rows of a CSV that share a unique ID into a single feature (designated with `-i` (usually representing a admin geography)); values in each row within that selected column will be grouped as nested properties within an object named after the column in the consolidated feature properties.
- `--groupby` can be used with `upload` or the `join` command to extract the hierarchy from a CSV and upload it to a space without geometries.
- with `join`, the data is uploaded and the virtual space with the geometry space is created in one step.
- `upload --groupby` is useful for updating the "data space" in a virtual space that has already been created. It can also be used to upload the grouped data before a virtual space has created with a space containing geometries matching geoIDs using `here xyz vs -a`

This is a complex feature and is best explained by example.

Imagine a CSV of election results where each row contains a precinct ID, candidate name, political party, and that party's percentage of the vote. Since there are multiple candidates running for office in each precinct, there would be multiple rows containing the same precinct ID, which is what we would need to join this data to precinct geometries on a map.

    precinct,candidate,party,vote_percentage
    1,Baker,Red,30
    1,Farmer,Blue,60
    1,Fisher,Green,10
    2,Woods,Red,60
    2,Waters,Blue,10
    2,Rivers,Green,30

However, if we tried to upload this CSV using `-i precinct`, Data Hub would consider each new feature with that unique ID to be an update of the existing feature which is not what we want.

Using `group_by`, we can designate `precinct` as the feature ID, and select the values of `party` to be the nested object. This would generate geojson similar to this

    {
      id: 1,
      geometry: null
      properties: {
        party: {
          Red: {
            candidate: Baker
            vote_percentage: 30
          },
          Blue: {
            candidate: Farmer
            vote_percentage: 60
          },
          Green: {
            candidate: Fisher
            vote_percentage: 10
          }
        }
      },
    {
      id: 2,
      geometry: null
      properties: {
        party: {
          Red: {
            candidate: Woods
            vote_percentage: 60
          },
          Blue: {
            candidate: Waters
            vote_percentage: 10
          },
          Green: {
            candidate: Rivers
            vote_percentage: 30
          }
        }
      }

This "data space" has no geometries, but using virtual spaces, it is simple to merge it on the fly with a second space that contains the precinct geometries with the same set of feature IDs.

You can upload and merge this CSV two ways, using `upload` and `join`.

    here xyz upload -f http://elections.xyz/results.csv -i precinct --groupby party --noCoords 
    here xyz vs -a csvSpace,geometrySpace

or

    here xyz join precinctGeometrySpace -f http://elections.xyz/results.csv -i precinctID --groupby party --noCoords 

This uploads the CSV into a space where the results for each precinct are consolidated in a GeoJSON feature with no geometry, and the results for each candidate / political party would be nested within a `party` object. With virtual spaces, these are easily merged with a space containing the precinct geometries with the same feature ID. The virtual space ID can then be visualized in a variety of mapping tools. More importantly, the space containing the nested CSV data can be updated each time it is updated by election officials using the `here xyz upload` command specified above, avoiding the work involved in a manual join with a large, nationwide set of geometries.

You can try this yourself with [this CSV of election results from the 2019 Canadian Federal Election](data/2019_canadian_federal_election_results.csv) and the shared Data Hub space `mo3sLwE3`, which contains polygons of Canadian electoral precincts.

    here xyz join mo3sLwE3 -f https://github.com/heremaps/xyz-documentation/raw/master/docs/cli/tutorials/data/2019_canadian_federal_election_results.csv -i District --groupby party --noCoords

Another example is COVID-19 data from the Covid Tracking project API.

    here xyz join xkRyxQl9 -f https://covidtracking.com/api/v1/states/daily.csv --noCoords -i state --groupby date

This will merge daily state testing data from March 2020 until today into a virtual space with xkRyxQl9, a shared space with US state geometries.

Another advantage of using Virtual Spaces is the ability to merge the data table into different spaces containing geometries with different properties or resolutions. Consider [this hexgrid of US states](https://s3.amazonaws.com/xyz-demo/scenes/xyz_tangram/index.html?space=UZ4LdhAn&token=AKj2021fSByO2JUUUQqx5QA&basemap=none&projection=mercator&demo=0&vizMode=range&buildings=1&pattern=&patternColor=%2384c6f9&points=15&lines=0&outlines=5&places=1&roads=1&clustering=0&quadCountmode=mixed&quadRez=4&hexbins=0&voronoi=0&delaunay=0&water=0&label=date%5B20200622%5D.positiveIncrease&property=date%5B20200622%5D.positiveIncrease&palette=viridis&paletteFlip=false&rangeFilter=1&sort=values&hideOutliers=false&pointSizeProp=&pointSizeRange=%5B4%2C20%5D&propertySearch=%7B%7D#4/40.54/-96.47).

Note that it is not recommended to use streaming / `-s` with `groupby` -- attempting simultaneous writes to the same property of the same feature can lead to potential API errors.
