# HERE XYZ and Microsoft Building Footprints

During the summer of 2018 Microsoft released a set of [building footprints](https://github.com/Microsoft/USBuildingFootprints) based on analysis of aerial imagery. In all, the Microsoft dataset consisted of 125,192,184 building footprint polygon geometries in all 50 US States in GeoJSON format.

This dataset formed the basis for an article, published on October 12th 2018, in the NY Times entitled “[A Map of Every Building in America](https://www.nytimes.com/interactive/2018/10/12/us/map-of-every-building-in-the-united-states.html).”

In early March 2019, Microsoft released a set of building footprints, using the same techniques, for all of [Canada](https://github.com/Microsoft/CanadianBuildingFootprints).

Since HERE XYZ is a location data management service, we decided to see what we could do with these large datasets.

We started with the source data from Microsoft and processed it so that users of XYZ could quickly create maps that
- don’t require coding or GIS experience
- don’t require large file uploads or downloads
- allow filtering of the source data to focus in on a particular part of the US and/or Canada
- supports maximum zoom in
- allows exploration of the source data.

Take a look [at a map created by XYZ Studio](https://xyz.here.com/viewer/?project_id=c9884248-dbda-4c2b-a45c-8a46d0c7d3fb) filtered to focus on selected ZIP codes in San Francisco.

This experience is possible because the data is in an XYZ Space, something that we think makes sense for most large geospatial datasets.

However, for those who want to get at the data directly, we have another option.

HERE Technologies worked with [geocode.earth](https://geocode.earth) to enhance the original Microsoft Building Footprint dataset to add admin attributes to each polygon. As a result, the enhanced dataset can be filtered by admin attribute to focus on the data of interest.

The data is available in two formats – GeoJSON and GeoJSONL. ([Learn more about GeoJSONL over at interline.io](https://www.interline.io/blog/here-cli-supports-geojsonl/).) It maintains the [ODbL license granted by Microsoft](https://github.com/Microsoft/USBuildingFootprints/blob/master/LICENCE-DATA). 

Both formats can be efficiently uploaded to HERE XYZ Spaces using the HERE CLI using the `-s` streaming option.

This geocoded dataset is also available in a shared XYZ Space, `XHmWfTCt`, which available using access tokens from your own HERE XYZ account.

You can use XYZ tags to preview and extract various sub-regional admin levels, including `neighborhood`,`locality`, and `county`, along with `street` and `postalcode`. Some demonstration maps are provided below.


| region | geojsonl | geojson | size | HERE XYZ map (tag,filter) |
|--------|----------|---------|------|------------|
| Alabama | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Alabama.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Alabama.geojson)| 2.29 GB |
| Alaska | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Alaska.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Alaska.geojson)| 0.11 GB |
| Arizona | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Arizona.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Arizona.geojson)| 2.59 GB |
| Arkansas | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Arkansas.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Arkansas.geojson)| 1.34 GB |
| California | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/California.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/California.geojson)| 11.33 GB |
| Colorado | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Colorado.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Colorado.geojson)| 2.06 GB |
| Connecticut | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Connecticut.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Connecticut.geojson)| 1.22 GB |
| Delaware | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Delaware.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Delaware.geojson)| 0.33 GB |
| District of Columbia | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/DistrictofColumbia.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/DistrictofColumbia.geojson)| 0.06 GB |
| Florida | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Florida.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Florida.geojson)| 6.89 GB |
| Georgia | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Georgia.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Georgia.geojson)| 3.60 GB |
| Hawaii | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Hawaii.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Hawaii.geojson)| 0.25 GB |
| Idaho | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Idaho.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Idaho.geojson)| 0.83 GB |
| Illinois | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Illinois.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Illinois.geojson)| 5.09 GB |
| Indiana | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Indiana.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Indiana.geojson)| 3.41 GB |
| Iowa | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Iowa.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Iowa.geojson)| 1.86 GB |
| Kansas | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Kansas.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Kansas.geojson)| 1.67 GB |
| Kentucky | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Kentucky.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Kentucky.geojson)| 2.13 GB |
| Louisiana | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Louisiana.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Louisiana.geojson)| 1.95 GB |
| Maine | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Maine.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Maine.geojson)| 0.77 GB |
| Maryland | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Maryland.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Maryland.geojson)| 1.60 GB |
| Massachusetts | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Massachusetts.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Massachusetts.geojson)| 2.16 GB |
| Michigan | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Michigan.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Michigan.geojson)| 4.88 GB |
| Minnesota | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Minnesota.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Minnesota.geojson)| 2.91 GB |
| Mississippi | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Mississippi.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Mississippi.geojson)| 1.33 GB |
| Missouri | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Missouri.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Missouri.geojson)| 3.14 GB |
| Montana | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Montana.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Montana.geojson)| 0.74 GB |
| Nebraska | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Nebraska.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Nebraska.geojson)| 1.14 GB |
| Nevada | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Nevada.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Nevada.geojson)| 0.95 GB |
| New Hampshire | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/NewHampshire.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/NewHampshire.geojson)| 0.57 GB |
| New Jersey | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/NewJersey.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/NewJersey.geojson)| 2.62 GB |
| New Mexico | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/NewMexico.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/NewMexico.geojson)| 0.99 GB |
| New York | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/NewYork.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/NewYork.geojson)| 5.12 GB |
| North Carolina | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/NorthCarolina.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/NorthCarolina.geojson)| 4.43 GB |
| North Dakota | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/NorthDakota.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/NorthDakota.geojson)| 0.55 GB |
| Ohio | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Ohio.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Ohio.geojson)| 5.72 GB |
| Oklahoma | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Oklahoma.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Oklahoma.geojson)| 1.91 GB |
| Oregon | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Oregon.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Oregon.geojson)| 1.78 GB |
| Pennsylvania | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Pennsylvania.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Pennsylvania.geojson)| 4.94 GB |
| RhodeIsland | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/RhodeIsland.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/RhodeIsland.geojson)| 0.39 GB |
| South Carolina | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/SouthCarolina.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/SouthCarolina.geojson)| 2.06 GB |
| South Dakota | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/SouthDakota.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/SouthDakota.geojson)| 0.66 GB |
| Tennessee | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Tennessee.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Tennessee.geojson)| 2.88 GB |
| Texas | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Texas.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Texas.geojson)| 9.72 GB |
| Utah | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Utah.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Utah.geojson)| 1.01 GB |
| Vermont | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Vermont.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Vermont.geojson)| 0.36 GB |
| Virginia | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Virginia.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Virginia.geojson)| 2.98 GB |
| Washington | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Washington.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Washington.geojson)| 2.95 GB |
| West Virginia | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/WestVirginia.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/WestVirginia.geojson)| 0.98 GB |
| Wisconsin | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Wisconsin.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Wisconsin.geojson)| 3.20 GB |
| Wyoming | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Wyoming.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Wyoming.geojson)| 0.36 GB |
| 🇨🇦 | | |
| Alberta | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Alberta.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Alberta.geojson)| 1.81 GB | [Edmonton (streets)](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&basemap=refill-dark&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=0&water=0&property=street#14/53.5356/-113.4659) |
| British Columbia | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/BritishColumbia.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/BritishColumbia.geojson)| 1.49 GB | [Victoria (neighbourhoods)](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&basemap=refill-dark&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=1&water=0&tags=locality%40victoria%2Clocality%40oak_bay&property=neighbourhood#14/48.4220/-123.3446) |
| Manitoba | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Manitoba.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Manitoba.geojson)| 0.71 GB | [Winnipeg (neighbourhoods)](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&basemap=refill-dark&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=0&water=0&tags=locality%40winnipeg&property=neighbourhood#14.166/49.8885/-97.1326) |
| New Brunswick | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/NewBrunswick.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/NewBrunswick.geojson)| 0.32 GB | [St. John](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&basemap=refill-dark&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=0&water=0&tags=county%40saint_john&property=street#15/45.2740/-66.0576) |
| Newfoundland and Labrador | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/NewfoundlandAndLabrador.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/NewfoundlandAndLabrador.geojson)| 0.22 GB | [Goose Bay](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&basemap=refill-dark&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=1&water=0&tags=county%40division_no._10&property=street#13.04/53.3087/-60.3465) |
| Northwest Territories | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/NorthwestTerritories.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/NorthwestTerritories.geojson)| 0.02 GB | [Yellowknife](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&basemap=refill-dark&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=0&water=0&tags=locality%40yellowknife&property=street#13/62.4522/-114.3799) |
| Nova Scotia | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/NovaScotia.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/NovaScotia.geojson)| 0.35 GB | [Halifax (downtown streets)](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&basemap=refill-dark&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=0&water=0&tags=street%40robie_street%2Cstreet%40robie_st%2Cstreet%40oxford_st%2Cstreet%40barrington_street%2Cstreet%40barrington_st&property=street#14/44.6497/-63.5908)|
| Nunavut | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Nunavut.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Nunavut.geojson)| 0.01 GB | [Iqaluit (streets)](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&basemap=refill-dark&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=0&water=0&tags=locality%40iqaluit&property=street#13.7/63.7488/-68.5106)|
| Ontario | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Ontario.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Ontario.geojson)| 3.71 GB | [Toronto (Yonge St)](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=1&water=0&tags=street%40yonge_street%2Cstreet%40yonge_st&property=locality#12.7/43.6967/-79.3880)
| Prince Edward Island | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/PrinceEdwardIsland.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/PrinceEdwardIsland.geojson)| 0.07 GB | [Charlottetown (streets)](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=0&water=0&tags=county%40queens&property=street#14/46.2572/-63.1252) |
| Quebec | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Quebec.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Quebec.geojson)| 2.23 GB | [Quebec City (neighbourhoods)](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=0&water=0&tags=locality%40quebec&property=neighbourhood#14/46.8072/-71.2363) |
| Saskatchewan | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/Saskatchewan.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/Saskatchewan.geojson)| 0.97 GB | [Regina (streets)](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=0&water=0&tags=locality%40regina&property=street#14/50.4515/-104.6072) |
| Yukon Territory | [geojsonl](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojsonl/YukonTerritory.geojsonl)| [geojson](https://s3.amazonaws.com/xyz-demo/msft-buildings-pelias/geojson/YukonTerritory.geojson)| 0.02 GB | [Whitehorse (streets)](http://geojson.tools/xyz_space_invader/?space=XHmWfTCt&token=AZAMz96qYi3wWNrtOfYFMFc&buildings=0&points=3&randomColors=0&lines=0&outlines=0&highlight=0&roads=0&water=0&tags=locality%40whitehorse&property=street#14.624999999999996/60.7211/-135.0556) |

(Hint: press `R` to toggle roads.)
           

