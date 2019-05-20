# XYZ Space Invader

XYZ Space Invader lets you inspect and analyze data, properties, and tags in an XYZ Space.

It uses the tile query feature of the Tangram.js API, Space Invader to read, count and store all tags in the vector tiles loaded for map display, saving a second query to the XYZ endpoint.

It can also read and analyze properties of features in the viewport, and dynamically assign color ranges based on their ranges (for numbers), or counts (of discrete values). It can calculate basic statistics on values in a space and use those to help display appropriate color ramps.

From the HERE XYZ `/statistics` endpoint, it reads the number of features and the size of the space along with the bounding box of the data, and attempts to center the map there (unless the map is already centered within the bbox).

Multiple color palettes can be applied to property ranges and ranks, and it is designed to be easy to add more.

Multiple basemaps are available. Basemap properties such as roads and buildings can be toggled on and off. The size of points and lines can also be increased and decreased to help visualize data sets of various density. 

XYZ Space Invader can show hundreds of thousands to millions of features, though this depends greatly on geographic density of data, zoom level, and kind and complexity of the geometries involved. In general you will be able to show more points and lines than polygons at any given zoom level.

## To get started

- [install the HERE CLI](https://www.here.xyz/cli/)
- upload a GeoJSON file, CSV or Shapefile to an XYZ Space, and add tags based on properties https://www.here.xyz/cli/cli/
- open XYZ Space Invader from your account using `here xyz show SPACEID -v`

Here are some examples to get you familiar with the interface and what Space Invader can do:

**2018 Gubernatorial California Primary results (LA Times)**

The Los Angeles Times gathers, normalizes and aggregates precinct level election data from counties in Southern California. Here is data for the 2018 Gubernatorial Primary from the 7500 precincts in Los Angeles County, Orange County, and San Diego County. 

_note: these URLs are based on the latest branch -- this will change, and the root URL (for now) is xyz-space-invader.netlify.com_

### X-ray mode

X-ray mode shows you an overview of the geometries in the space. This is especially useful when you have overlapping geometries, or different types of geometries, or you just want to see the coverage of the data set.

https://min-max--xyz-space-invader.netlify.com/?space=ylnRzWDL&token=AOsE9k2EdCdT8lEX12PDZ38&basemap=refill-dark&buildings=0&outlines=1&roads=1&water=0&colors=xray#11.6/34.0256/-118.3172

_Note: You can toggle polygon outlines on and off (either press 'o' or click on 'outlines' in the top left panel)_

### Hash mode

This generates unique colors based on a hash of all a feature's properties. This is useful for distinguishing adjacent or nearby features. Note however that this does not guarantee that similar colors will not be adjacent to each other. 

https://min-max--xyz-space-invader.netlify.com/?space=ylnRzWDL&token=AOsE9k2EdCdT8lEX12PDZ38&basemap=refill-dark&buildings=0&outlines=1&roads=1&water=0&colors=hash&points=0&lines=0&highlight=0&property=winner&palette=viridisInferno&paletteFlip=false#13.795833333333333/34.0581/-118.2976

## Coloring features by property

There are several ways to generate colors by the value of an individual property. Once the mode is selected, click on a property in the center left pane to analyze it.

### Property

This is the most primitive -- it generates a simple color hash of the selected property value.

https://min-max--xyz-space-invader.netlify.com/?space=ylnRzWDL&token=AOsE9k2EdCdT8lEX12PDZ38&basemap=refill-dark&buildings=0&outlines=1&roads=1&water=0&colors=property&points=0&lines=0&highlight=0&property=winner&palette=viridisInferno&paletteFlip=false#11.820833333333333/34.0372/-118.2583

Color choices are consistent for any specific property value.

### Range

This is ideal for non-discrete numerical values, and will apply a color palette across from the min to max values of the property seen in the viewport.

https://min-max--xyz-space-invader.netlify.com/?space=ylnRzWDL&token=AOsE9k2EdCdT8lEX12PDZ38&basemap=refill-dark&buildings=0&outlines=1&roads=1&water=0&colors=range&points=0&lines=0&highlight=0&property=gov_votes&palette=viridisInferno&paletteFlip=false#11.050000000000011/34.0306/-118.1441

You can choose between several different color palettes, and you can also "flip" each palette, depending on if you want to emphasize the lower or higher values with 'brighter' values.

Individual values of properties in the viewport can be sorted either by count or by value. (Note that this can be a very long list, depending on the kind or size of the data.)

Note that the color of an individual feature may change as you pan or zoom the map as different `min` and `max` values become visible in the viewport. This allows better inspection of otherwise subtle differences between "local" values, especially when "global" `min` and `max` is extreme compared to local values. (However, we recognize that being able to compare local values to global `min` and `max` values is also necessary. This will come in a later release.)

Some useful visualizations include:

- sort by total votes (range)
- sort by votes per candidate (range)
- sort by winner (rank)

#### Statistics

Basic statistics are generated for the values of the selected property, including, min, max, median, mean, standard deviation, and sigma.

A dynamic decile histogram is generated in order to give you an idea of how the data is distributed in the viewport.

A quick note on outliers: In many datasets, a few outliers can cause a max so high that the majority of features get 'squished' into the opposite end of the color palette. You can see the high and the low, but there may be thousands of values in the 'low' colors. For the time being, we limit the "display" max to be four sigmas above the mean. This in effect 'squishes' the outliers into the high end of the color range, while the majority of the values get more room in the color palette. More fine-grained controls for manipulating the min and max values to handle value ran

The benefit of this approach can be seen in this dataset of AirBnB listings:

https://min-max--xyz-space-invader.netlify.com/?space=40ILezn0&token=AOsE9k2EdCdT8lEX12PDZ38&basemap=refill-dark&buildings=0&outlines=1&roads=1&water=0&colors=range&points=3&lines=0&highlight=0&tags=listings&property=price&palette=viridis&paletteFlip=false#14.200000000000005/34.0837/-118.3770

Most properties in the Los Angeles area are under $250 a night, but a few listings are as high as $10,000 or even $25,000. By considering a 'display max' closer to the median and mean, a meaningful spread of colors can be shown, as opposed to just two for very high and very low.

More interesting views of AirBnB listings data include:

- single rooms vs whole houses (rank or property)
- prices (range)
- names (rank)


US Census data by income (California and San Francisco)

https://min-max--xyz-space-invader.netlify.com/?space=zK3K5S6b&token=AOsE9k2EdCdT8lEX12PDZ38&basemap=none&buildings=0&points=1&lines=1&outlines=0&highlight=0&roads=0&water=0&colors=range&tags=zip&property=B19013001&palette=viridisPlasma&paletteFlip=false#7.775416573527989/37.645/-121.339

Filter by tags to show data by

- county
- zip code
- census tract (SF only)

The income of certain counties and zip codes is so far above the average that it triggers the 4-sigma filter.


## Tags (WIP)

Excavation Permits in San Francisco, by rank:

![sf-excavation-rank-tagged-agent](sf-excavation-rank-tagged-agent.png)

https://min-max--xyz-space-invader.netlify.com/?space=ZLvdcZgi&token=AOsE9k2EdCdT8lEX12PDZ38&basemap=refill-dark&buildings=0&outlines=1&roads=1&water=0&colors=rank&points=0&lines=0&highlight=0&property=Permit_Reason&palette=viridis&paletteFlip=false#12.973739133520723/37.7546/-122.4372

AirBnB listings in the Hollywood Hills:

![airbnb-hollywood_hills-range-tags](airbnb-hollywood_hills-range-tags.png)

https://min-max--xyz-space-invader.netlify.com/?space=40ILezn0&token=AOsE9k2EdCdT8lEX12PDZ38&basemap=refill&buildings=0&outlines=1&roads=1&water=0&colors=range&points=0&lines=0&highlight=0&tags=neighbourhood%40hollywood_hills%2Blistings&property=price&palette=viridis&paletteFlip=true#13.779166666666672/34.1220/-118.3403

