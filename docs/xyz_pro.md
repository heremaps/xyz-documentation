# HERE XYZ Pro (beta)

## Property Search

Property Search indexes data in your XYZ spaces and lets you use the API to search by property value.

This is currently available in the CLI and the API.

More details are available in the [CLI documentation](../cli/cli.md#property-search) and the [Swagger API `/search` endpoint](https://xyz.api.here.com/hub/static/swagger/#/Read%20Features/searchForFeatures).

## Virtual Spaces

Virtual Spaces let you group or associate geospatial features in multiple spaces and present them as a single space.

`group' acts as an alias, presenting multiple XYZspaces via a single space ID.

`associate` merges feature properties from one XYZ space into features in a second space based on feature ID matches. This allows you to import statistical data and merge it with pre-existing polygons on the fly.

This is currently available in the CLI and API. More details are available in the [CLI documentation](../cli/cli.md#virtual-spaces)


## Hexbins

Hexbins are a data simplification method that makes it easier to visualize large datasets of point features at low zoom levels (continent, country, state/province). A series of hexagon grids are created and the points that fall inside each are counted and written to a new space, and statistics are calculated across the hexbin grid.

These hexagons, their centroids, and their statistics can be quickly displayed in place of the raw data that might overwhelm a renderer. Default colors indicating relative "occupancy" are generated for convenience of display.

Hexbins are tagged by zoom level and width and type, makeing it easy to extract one set from the hexbin space for display and comparison.

You can learn more about hexbins and how to display them in this tutorial, the [CLI documentation](../cli/cli.md#hexbins)

## Rule Based Tags

Create tags using conditional rules based on the values of properties in features. This makes it easy to view and extract data from your XYZ sapce.

## Schema Validation

Apply a schema validation json file to space

The schema definition can be in the form of a web address or a local schema json file.

!!! note "Schema validation is not transactional (yet)"
    The schema validation will be non transactional (Transaction = FALSE) i.e. upload all the objects which passes schema definition and display the list of objects rejected.

## Change Log
