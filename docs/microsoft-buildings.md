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

Take a look at an [XYZ Studio map](https://xyz.here.com/viewer/?project_id=c9884248-dbda-4c2b-a45c-8a46d0c7d3fb) with the dataset (just San Francisco)

To explore the data yourself <show -v with SpaceID>

This experience is possible because the data is in an XYZ Space – something that we think makes sense for most large geospatial datasets.

However, for those who want to get at the data directly, we have another option.

HERE Technologies worked with geocode.earth to enhance the original Microsoft Building Footprint dataset to add admin attributes to each polygon. As a result, the enhanced dataset can be filtered by admin attribute to focus on the data of interest.


The data is available in two formats – GeoJSON (link to S3 bucket) and GeoJSONL (link to S3 bucket).

Both datasets can be efficiently uploaded to HERE XYZ Spaces using the HERE CLI.
