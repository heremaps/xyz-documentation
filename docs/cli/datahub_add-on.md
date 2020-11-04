# HERE Data Hub Add-on

>#### Note
>
>These features require the [Add-On plan](https://developer.here.com/pricing) to use Add-on features. To sign up for access these features, go to: [developer.here.com/pricing](https://developer.here.com/pricing)

## Property Search

Property Search indexes data in your Spaces and lets you use the API to search by property value.

This is currently available in the CLI and the API. Property Search is available for all Spaces with less than 10,000 features. In order to use Property Search on more than 10,000 features and for all properties you want, you'll need a Data Hub Add-on account.

More details are available in the [CLI documentation](basic-features.md#property-search) and the [Swagger API `/search` endpoint](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/searchForFeatures).

## <a name="virtual-spaces">Virtual Spaces</a>

Virtual Spaces let you group or associate geospatial features in multiple Spaces and present them as a single Space.

`group` acts as an alias, presenting multiple Data Hub spaces via a single Space ID.

`associate` merges feature properties from one Data Hub space into features in a second space based on feature ID matches. This allows you to import statistical data and merge it with pre-existing polygons on the fly.

This is currently available in the CLI and API. For more details, see the [CLI documentation](add-on.md#virtual-spaces)

## GIS Functions

Use the CLI to calculate the area of a polygon or length of line and save it as a new property. Generate centroids of polygons. Create Voronoi polygons or Delaunay triangles from sets of points. For more details, see the [CLI documentation](add-on.md#gis)

## Hexbins

Hexbins are a data simplification method that makes it easier to visualize large datasets of point features at low zoom levels (continent, country, state/province). A series of hexagon grids are created and the points that fall inside each are counted and written to a new space, and statistics are calculated across the hexbin grid.

These hexagons, their centroids, and their statistics can be quickly displayed in place of the raw data that might overwhelm a renderer. Default colors indicating relative "occupancy" are generated for convenience of display.

Hexbins are tagged by zoom level and width and type, making it easy to extract one set from the hexbin space for display and comparison.

You can learn more about hexbins and how to display them [in this tutorial](tutorials/cli_hexbins.md) and the [CLI documentation](add-on.md#cli-hexbins)

## Rule Based Tags

Create tags using conditional rules based on the values of properties in features. This makes it easy to view and extract data from your Data Hub space.

## Schema Validation

Apply a schema validation JSON file to space to ensure that only valid data is uploaded to a Data Hub space.

The schema definition can be in the form of a web address or a local schema JSON file.

> #### Schema validation is not transactional
>
> The schema validation will be non-transactional (`Transaction = FALSE`) and will upload all the objects which pass schema definition and display the list of objects rejected.

## Activity Log

See what's been written, modified, and deleted in a Space. The changes are written to a second Space, with options to show

- `FEATURE_ONLY` (default): Just the full new version of the feature, with the ID moved. No diff to previous.
- `DIFF_ONLY`: Head (newest object) is the full feature. All older versions are only a Diff to the successor. In order from newest to oldest: Obj1: Newest, full Feature + Diff to Obj2-> Obj2: Diff to Obj3 -> Obj3: Diff to Obj4 -> Obj4 …
- `FULL`: Every feature in Activity Log is full feature + Diff to previous.
