# Release Notes for Data Hub

The most up-to-date release notes are available on GitHub.

- https://github.com/heremaps/xyz-hub/releases
- https://github.com/heremaps/here-cli/releases


### HERE CLI v 1.5.1

2020-08-04

This minor version brings support for HERE Studio projects:
- `here studio list` - lists your HERE Studio projects
- `here studio show <project-id>` - launches a published project in the browser
- `here studio delete <project-id>` - to delete a project


### HERE CLI v 1.5.0

2020-06-22

Just in time for summer, v1.5.0 of the Data Hub CLI is here with new features and bug fixes that will help your geospatial data kick back and relax in a comfortable space. 

## 🔤🔤🔤 CSV Group by 🔢🔢🔢

`--groupby columnName` consolidates multiple rows of a CSV that share a unique ID into a single feature (designated with `-i` (usually representing a admin geography)); values in each row within that selected column will be grouped as nested properties within an object named after the column in the consolidated feature properties. 

`-groupby` can be used with `upload` or the `join` command to extract the heirarchy from a CSV and upload it to a space without geometries. 
- with `join`, the data is uploaded and the virtual space with the geometry space is created in one step.
- `upload --groupby` is useful for updating the "data space" in a virtual space that has already been created. It can also be used to upload the grouped data before a virtual space has created with a space containing geometries matching geoIDs using `here xyz vs -a`

This feature is best illustrated by election data, census data and time series data. One example is COVID-19 data from the Covid Tracking project API.

    here xyz join xkRyxQl9 -f https://covidtracking.com/api/v1/states/daily.csv --noCoords -i state --groupby date

This will merge daily state testing data from March 2020 into a virtual space with `xkRyxQl9`, a shared space with US state geometries.

## Date tags and properties

We've added `--dateprops` to the `--date` option, meaning we now let you save your time slices as new properties in the feature as well as tags -- these are prefixed by `xyz_propertyname_`.

Also, while converting the date string, we no longer add a time zone offset to the ISO 8601 timestamp as this caused problems when the data wasn't collected in your timezone.

## 📜💾⭐️ CLI history ⭐️💾📜

You're working with that complex dataset, and you just created the _perfect upload command_. It is so good, the data in your space is excellent, and your map is happy. Then you go back to update that space six months later… and you have completely forgotten the options. Was it `-i`? What chunk size did I use? How did I make that tag?

Well, never fear, `upload --history` is here. It automatically records the last three upload commands in the space definition, and it even lets you `--save` one as a `--fav`.

If you've done at least one upload to a space, you can recall it using an interactive menu:

    here xyz upload spaceid --history

You can pick an upload command to save with `--history save`, and then re-run that particular command using `--history fav`. You can also `--clear` your history.

## 🚚🚚🚚 Batch upload 🚢🚢🚢

If you have a folder full of geospatial files, you can upload them all to a space in one command with `--batch`. Just specify the directory with `-f` and the filetype after `--batch`: `geojson`,`geojsonl`,`csv`, `shp`,`json`, or `gpx` and watch your files fly into Data Hub.

While uploading shapefiles,`--batch` will inspect directories in that specificed directory and look for `.shp` and all the other files you get when uncompressing a zipped shapefile. (This is handy, say when you've downloaded say 50 state shapefiles from the US Census website. We've all been there.)

## 🔍🔍🔍 Data Hub Console 🔎🔎🔎

Both `config` and `token` have a new `--console` option that opens up the new Data Hub console in a web browser.

## 🐛🐞🦟 Bug fixes and other enhancements 🛠🔧🔩

- inline help was improved, along with online documentation
- `here xyz list --filter` does a better job of handling null title and description fields
- GeoJSON features that are not in a `featureCollection` can once again be uploaded
- `config -r` now outputs properly formatted `json`
- confirmation prompt added to `config --shared true`
- `join -k` was changed to `join -i` to be more consistent with `upload -i`
- we fixed a bug in activity log creation -- we were sending state as a string, but the API expected a number
- we fixed some issues while streaming `voronoi` and `delaunay` polygons
- `show -r` no longer wraps `geojsonl` output in a `featureCollection`
- we set `skipCache=true` for `/statistics` and `/space` `GET` calls so you get the latest and greatest metadata



### HERE CLI v 1.4.0

2020-04-30

1.4.0 is here, and so are you! In addition to various bug fixes and security updates, we also bring you normalized date strings, better list options, .json import, and easier export!

### :alarm_clock::timer_clock::date: Date strings, time stamps, and date tags :mantelpiece_clock::hourglass::stopwatch:
If you have a field or property that has a date-like field or a timestamp, you can normalize it using `--date propertyname`. We will try to convert it to an ISO 8601 friendly value and save it in `xyz_iso8601_propertyname`, along with a unix seconds timestamp, `xyz_timestamp_propertyname` for your time-slicing needs. (UTC for now.)
If you add the `--datetag` option, we will calculate human-friendly tag buckets such as `year@2020`, `month@april`, `weekday@thursday`, `week@21`. You can also limit which tags you want using arguments like `--datetag month`. (Again, these are UTC-based for now, timezones are hard)..

### :sunglasses::dark_sunglasses::headphones: Find that space using filters :headphones::dark_sunglasses::sunglasses:
You have a lot of spaces. They are all important and you love them equally. But you need to find that special one. Why not use here `xyz list --filter theoneyouarelookingfor` to narrow down your list based on what you added to the title and description?

### {{{JSON import}}}
Just like a square is a rectangle, but a rectangle is not a square, GeoJSON is JSON, but JSON is not GeoJSON. But if your JSON file contains (non-nested) lat/lon coordinates, you can now import those non-geojson json files just like you can with a CSV.

### :rocket::satellite::dizzy: Get more out of your space :dizzy::satellite::rocket:
• `show --raw` used to be limited to 5000 features, but if you need more, try --all (this can get long, and you can redirect to a file using `>`)
• you can also export to GeoJSONL via show `--raw` using, you guessed it, `show --raw --geojsonl`

Take care and we'd love to hear from you in issues (because we have issues).

### HERE CLI v 1.3.1 

2020-03-24

This minor release repairs options in the `show` command.

---

### HERE CLI v 1.3.0

2020-03-19

Bugs have been tracked down and features have been hoisted up. We clarified lots of the inline documentation -- check it out with `-h`. And as for the new features:

🔎🗺🔍 Spatial Search  🔎🌏🔍 

You can now select a set features within a radius around a point, using `--spatial` in the `show` command:

    here xyz show spaceID --spatial --center "37.74,-122.43" --radius=100 -w

And as if that were not exciting enough, you can _also_ select features from one space that fall within a polygon from another space! Imagine you had a space that had postal codes as the feature ID:

    here xyz show space_with_points --spatial --feature space_with_zip_codes,94104 -w


➕➕➕ CSV Join  ➕➕➕

Virtual Spaces are fun, but it's sometime laborious to get the data you need into XYZ, especially when it's a CSV table with data and IDs but no coordinates (think Census tables). But now you can use the new `join` command to dynamically associate a CSV to space with geometries with those same IDs!

    here xyz join space_with_geometries -f table_with_ids.csv -k unique_id

This means you can pour the properties from many CSVs into the polygons of one existing space and access them through the new virtual space IDs.

Note that uses the same CSV import options (delimiter, quotes, string-fields).

You can also update a CSV space and the Virtual Space will dynamically update.

🏃‍♀️🏃‍♂️👟 Import GPX files  🚴‍♀️🚴‍♂️🚲

If you run or bike, you probably know what GPX traces are, and you'll be excited to know we can now import them into XYZ aka Data Hub! Just reference the file in `here xyz upload my.gpx` and we will convert it into delicious GeoJSON using @roberto-butti's sweet code. Thanks for the [PR](https://github.com/heremaps/here-cli/pull/179), Roberto!


🎉🎉🎉Other interesting enhancements 🎉🎉🎉

- Features are now written to XYZ spaces using `PATCH` instead of `POST`, meaning you can make more granular edits to your features.

- It turns out point field columns are not always written as `(lat,lon)` but sometimes are `(lon,lat)` and we can now deal with that.

- You can now add `--token` to most commands, and presuming you have the appropriate permissions, you can do things to/with your friends' spaces.

- We are now using the d3-Delaunay library for Voronoi polygons.

- We check for troublesome characters in tags that break URLs, like `&` and `+` and `,`

- Users can now adjust the `cacheTTL` of a space using `config --cacheTTL`

- We are deprecating `here xyz describe` since `here xyz config --stats` does a much better job.

Thanks for watching, and let us know what you see in the issues!

---

### HERE CLI v 1.3.0

Bugs have been tracked down and features have been hoisted up. We clarified lots of the inline documentation -- check it out with `-h`. And as for the new features:

🔎🗺🔍 Spatial Search  🔎🌏🔍 

You can now select a set features within a radius around a point, using `--spatial` in the `show` command:

    here xyz show spaceID --spatial --center "37.74,-122.43" --radius=100 -w

And as if that were not exciting enough, you can _also_ select features from one space that fall within a polygon from another space! Imagine you had a space that had postal codes as the feature ID:

    here xyz show space_with_points --spatial --feature space_with_zip_codes,94104 -w


➕➕➕ CSV Join  ➕➕➕

Virtual Spaces are fun, but it's sometime laborious to get the data you need into XYZ, especially when it's a CSV table with data and IDs but no coordinates (think Census tables). But now you can use the new `join` command to dynamically associate a CSV to space with geometries with those same IDs!

    here xyz join space_with_geometries -f table_with_ids.csv -k unique_id

This means you can pour the properties from many CSVs into the polygons of one existing space and access them through the new virtual space IDs.

Note that uses the same CSV import options (delimiter, quotes, string-fields).

You can also update a CSV space and the Virtual Space will dynamically update.

🏃‍♀️🏃‍♂️👟 Import GPX files  🚴‍♀️🚴‍♂️🚲

If you run or bike, you probably know what GPX traces are, and you'll be excited to know we can now import them into XYZ aka Data Hub! Just reference the file in `here xyz upload my.gpx` and we will convert it into delicious GeoJSON using @roberto-butti's sweet code. Thanks for the [PR](https://github.com/heremaps/here-cli/pull/179), Roberto!


🎉🎉🎉Other interesting enhancements 🎉🎉🎉

- Features are now written to XYZ spaces using `PATCH` instead of `POST`, meaning you can make more granular edits to your features.

- It turns out point field columns are not always written as `(lat,lon)` but sometimes are `(lon,lat)` and we can now deal with that.

- You can now add `--token` to most commands, and presuming you have the appropriate permissions, you can do things to/with your friends' spaces.

- We are now using the d3-Delaunay library for Voronoi polygons.

- We check for troublesome characters in tags that break URLs, like `&` and `+` and `,`

- Users can now adjust the `cacheTTL` of a space using `config --cacheTTL`

- We are deprecating `here xyz describe` since `here xyz config --stats` does a much better job.

Thanks for watching, and let us know what you see in the issues!

---

### XYZ Hub - 1.3.0

✨ **CHANGED** ✨
- 4 flavours of the OpenAPI specification are now generated during the service start: stable, experimental, contract and full.
- The connector protocol version is updated to version 0.2.0. All feature metadata values are set by Hub.
- GeometryCollection is rejected as an input in Hub.

🔨 **FIXED** 🔧
- Fixed an issue with the initialization of connectors

---

### XYZ Hub - 1.2.1
🔨 **FIXED** 🔧
- Fix an error when updating a feature, using the query parameters addTags or removeTags, and providing a body with a feature, which does not include tags.

---

### XYZ Hub - 1.2.0

✨ **CHANGED** ✨
- For all POST, PUT and PATCH requests to modify features: when a feature input does not contain any changes, compared to the latest version of the feature, an update operation will not be executed. If a feature was not changed, the existing version of the object is included in the response, but its ID is not in any of the lists with inserted or updated features.

🔨 **FIXED** 🔧
- Fixed that the createdAt value can be overwritten by the value in the input.

--- 

### XYZ Hub - 1.1.0

✨ **ADDED** ✨
- HTTP connectors: Add new HTTP remote function client to allow communication with storage connectors over HTTP.
- Add versioning in the connector event protocol

🔨 **IMPROVED** 🔧
- DynamoDB can be configured as a default storage for connectors and spaces
- Add CORS support for static resources to allow loading of the OpenAPI specifications from OAS based applications hosted on a different domain.
- Extend the list of error codes in the ErrorResponse event that the connector can send to XYZ Hub. New codes are CONFLICT, FORBIDDEN and TOO_MANY_REQUEST.
- The default storage connector ID is now configurable.
- Switch from SLF4J to Log4J
- The execution trigger for statistic is move from the PSQL connector into the database.

🐜 **FIXED** 🐜
- Fix the response status code when the storage is set to null.
- Connector is now re-initialized correctly in all cases when its config was changed.
- Fix a potential blocking call to a ScheduledExecutorService.
- Fix an error when updating a space without any changes. 
- The space volatility was incorrectly calculated when the sliding window was zero.

---

### XYZ Hub - 1.0.1

✨ **ADDED** ✨
- Fix replacing feature with UUID

🔨 **CHANGED** 🔧
- Switch to semantic versioning 2.0.0. More information at https://semver.org/

---

### XYZ Hub - 2019.49.07 / 2019-12-05

✨ **ADDED** ✨
- XYZ Hub will auto configure the cache profile for a space.
- New property cacheTTL to manually set the cache time to leave or turn off the auto cache profile for a space.

---

### XYZ Hub - 2019.48.01 / 2019-11-29

✨ **ADDED** ✨
- Advanced spatial requests by any geometry with or without proximity-radius

---

### HERE Studio - release 1.5.7 (November 28, 2019)

✨ New ✨
• Outstanding Map Experience! Upgraded to xyz-maps 0.10.0-next10. Check it out now!

---

### HERE XYZ Studio - release 1.5.6 (November 19, 2019)

✨ NEW ✨
• Rich media content support in Studio and Viewer. Now you can view video, image, sound files rendered within cards by providing the link.
• New exciting color picker to select from wide ranges of colors.


🔨 IMPROVED 🔧
• Icon dropdown with image of icons.
• Show zoom levels in Tangram.
• Show and save property names as is uploaded by user.

🐜 FIXED 🐜
• Filter property bug fixes.
• Handling of new-line break in tangram while applying style rule.
• Performance Improvement fixes.
• Other minor bug fixes.

---

### XYZ Hub - 2019.47.01 / 2019-11-20

✨ **ADDED** ✨
- Add option to return the centroid of the hexagons as a feature geometry, for the hexbin
    clustering mode.
    
🔨 **CHANGED** 🔧
- Update the format of the MVT response: nested properties are serialized as JSON.

---

### XYZ Hub - 2019.44.03 / 2019-10-31

✨ **ADDED** ✨
- Add property contentUpdateAt indicating the last time the content of the space was updated.
- Add property readOnly to indicate, if the space is accessible only for read operations.
    The service will respond with 405 Method Not Allowed, when writing features in the space,
    when the flag is set.
- Add an option to receive feature Ids by setting accept header to application/geo+json for delete 
    requests.

🔨 **CHANGED** 🔧
- Update the ETag to a 128-bit hash value
- The property "features" is now always included in FeatureCollection responses.
- Move the global searchable field to the properties section of the statistics response.

🐜 **FIXED** 🐜
- Fix incorrect bounding box for some spaces in the statistics response.
- Fix inconsistent status codes for delete requests.

---

### Studio release 1.5.4

Exciting way to Filter your Properties! Add Data is more fun now. 

✨ **NEW** ✨

* Property Search and Filters

🔨 **IMPROVED** 🔧

* Add Data side panel redesign

🐜 **FIXED** 🐜

* Error popups now redirect to the correct feedback page

---

### XYZ Hub - 2019.41.01 / 2019-10-10 

✨ **ADDED** ✨

- Add possibility to configure, which properties are searchable in a space.
- Add possibility to retrieve the features of a space clustered as hexbins.
- Activate browser caching for space cacheTTL property larger than 0.

---

### Studio and Viewer Release 1.5

New Feature: Exciting User Onboarding! Signups and Login is more fun now. Check it out!

✨ **NEW** ✨

* New user onboarding.
* Styling of maps with the color property in your dataset.
* Don't worry If a color property does not exist, we have selected best colors to style your map and make it visually more appealing.


🔨 **IMPROVED** 🔧

* UX/UI improvements.
* Under the hood, performance improvements.


🐜 **FIXED** 🐜

* Fixed an issue of side panel UI distortion while using color palette.
* Fixed an issue of NaN being displayed in Account Dashboard.
* Other minor bug fixes.

---

### XYZ Hub - 2019.37.01 / 2019-09-09

🔨 **CHANGED** 🔧
- The values createdAt and updatedAt of the space definition are now public and visible in single space responses and space list responses.
- A new error response code `513 Response Payload Too Large` is sent to the clients for responses which are too large for AWS API Gateway.
- The URI length limit for requests to the API is increased from 4K to 10K.

🐜 **FIXED** 🐜
- Resolve an issue that the space is not stored, when a preprocessor returns a space definition without a modification.

---

### XYZ Pro Features Beta Release 1.0.1

This rather significant release provides access to exciting new features in the API:

🔎🔎🔎 **Property Search** 🔍🔍🔍

Property Search allows users to filter data by requesting features from the XYZ API based on the values of their properties. Only need buildings taller that 100 feet, or just addresses in one county? The XYZ Hub will automatically track and index these properties as you upload them, making it easy and fast to download just what you need.


You can access property search with -s and operators -- `here xyz show spaceID -s "p.property_name>value"`


Note that in a url, the arguments are `=, !=, =gt=, =gte=, =lt=, =lte=`.


Properties are automatically indexed depending on their count and value.


✂️✂️✂️ **Property Filter** ✂️✂️✂️

We've all been there -- you have a lot of properties in your features. Too many, probably. If only you could somehow... filter them, and only return what you need with your geometry.


We're here for you with `-p` -- even if you have 100 properties, `here xyz show -p p.property1,p.property2` will just return a feature with a geometry and those properties you carefully select.


❇️❇️❇️ **Virtual Spaces** ❇️❇️❇️

Virtual Spaces give users access to multiple spaces with one ID. Group lets you bundle your spaces together, and changes get written back to their original spaces. Associate lets you make your own personal edits to a shared space or one with public data, merging the properties of objects with the same feature ID.


`here xyz virtualize -a|-g space1,space2`


📐📐📐 **Schema Validation** 📏📏📏

Everyone needs validation, and so does your data. Upload a JSON Schema file to describe your data formats and ensure quality data in your XYZ Spaces.


`here xyz create -s schema.json`


⬢⬡⬢ **Hexbins** ⬢⬡⬢

Hexbins are a data simplification method that makes it feasible to visualize large datasets at low zoom levels (continent, country, state/province). A series of hexagon grids are created and the points that fall inside each are counted and written to a new space. These hexagons or their centroids can be quickly displayed in place of the raw data.


Learn more via `here xyz hexbin -h`


Note: reading from and writing to another user's space, and reading via bbox, are works in progress.


🔧🔧🔧 **Space Configuration** 🔧🔧🔧


Behold the new here xyz `config` command which not only lets you get data and statistics about your space, but edit and make changes to space metadata.

`here xyz config spaceID`

`here xyz config spaceID --stats`

---

### Studio and Viewer Release 1.4

New Feature: try out a new map renderer! Tangram is now available for published XYZ Studio maps.

**TL;DR:**

* Tangram map renderer.
* Re-ordering of geometries.

✨ **NEW** ✨

* Along with the minimalist HERE map renderer, we have have new Tangram map renderer.
* Tangram viewer provides new details like admin and street labels, as well as textures to the basemap.
* You can now reorder your geometries.
* Added two new basemap styles.

🔨 **IMPROVED** 🔧

* UX/UI improvements.
* Under the hood, performance improvements.

🐜 **FIXED** 🐜

* Fixed an issue of district not being shown in cards and style groups for Microsoft building footprint data.
* Other minor bug fixes.

---

### Studio and Viewer release 1.1.1

The XYZ Studio is all set to get you hooked on to the public datasets staring with Microsoft building footprint data.

**TL;DR:**

* You can now add Microsoft building footprint data to your project. You can also filter the data using tags.
* You can add copyright information in your maps.
* Studio login page just got interesting.

✨ **NEW** ✨

* Most awaited Microsoft building footprint data (US and Canada) is made available in studio. You can filter the data using tags. You can also style them as you want.
* Support for large datasets by using multiple space providers.
* You can have your own copyright information to be shown in maps. You can anytime edit or delete them in project dashboard or data hub.
** Added a new basemap style which we call a spring basemap. 😉 Right in time for the spring.
** Unsplash images added to the studio login page.
** Small animations in side panel and project thumbnails.

🔨 **IMPROVED** 🔧
* UX for the plan details page of account dashboard.
* Project dashboard thumbnails.
* Other minor UI improvements.

🐜 **FIXED** 🐜
* Fixed an issue of multiple plans get overplayed in footer of studio account dashboard.
* Other minor bug fixes.

---

### Studio Release 1.0.2

This one's all about the improved user experience! We've also released fixes for usage statistics in account dashboard.

* We've added more visual cues in billing section of account dashboard.
* We've made copy changes in studio to reflect the term Database Storage everywhere.

🔨 **IMPROVED** 🔧
* UI/UX of Account Dashboard
* UI/UX of Site Tour
* Other minor UI improvements.

🐜 **FIXED** 🐜
* Studio account dashboard will now show accurate usage converted into percentage. Note: the usage isn't tracked in real-time, so your numbers won't change immediately as you are working with data in Studio.
* You will now experience appropriate messages for your usage statistics. We have taken care of pay-as-you-grow users as well.
* Other minor bug fixes.

🔮 **COMING SOON** 🔮
* Microsoft building footprint data integration with XYZ Studio

---

### XYZ Maps 0.9.31 (2019-4-4)

:lower_left_crayon: **Editor** :lower_left_crayon:
* fixed: place drawingboard at the very top, regardless of link-layer's presence

:radio_button: **Core** :radio_button:
* fixed: only first tag is sent if url parameters are changed for SpaceProvider

:control_knobs: **Plugins** :control_knobs:
* added: support for 1024 pixel tileSize in MVTLayer

:hammer_and_pick: **General** :hammer_and_pick:
* workaround: (timing critical) in case of display got destroyed and listener is waiting for async execution.

:desktop_computer: **Display** :desktop_computer:
* added: UI support for expandable copyright information
* added: trigger "resize" event on map resize

:gear: **Common** :gear:
* added: Map.forEach(...)
* fixed: delete item in Set has no effect

---

### Studio Release 1.0.0
We’ve reached version 1.0.0 and are GA (Generally Available) now! This release introduces an account dashboard as well as a new site tour to guide first-time users

*TL;DR:* 
- Introducing the Studio account dashboard, a place to view your plans and track your usage. 
- We’ve added tours to guide you through the Studio dashboard and project interfaces. 
- We’ve removed shapefile upload support, and now provide a link to a tutorial on using the CLI to upload shape files. 
- Feedback and issue reporting is now available in published projects.

✨ **NEW** ✨

- We’ve added a site tour that will walk Studio newcomers through the dashboard, and another quick tour of their first project.
- Introducing the account management dashboard! This new dashboard, which you can access by clicking on your username in the top right corner, is a visual representation of your plan usage. Usage is tracked in two ways: data storage and data transfer.
  - If you have multiple plans, you’ll see a list and some summary stats about your usage. You can click on an individual plan to view more specifics about usage associated with that plan.  
  - If you only have one plan, you’ll be taken directly to the plan details.
  - Note: the usage isn’t tracked in real-time, so your numbers won’t change immediately as you are working with data in Studio.

📌 **Special Bulletin: More info on Plans**
> Customers can now get access to HERE XYZ under a Freemium Plan.  <br/><br/>
> This Freemium Plan entitles HERE XYZ customers to the following:  
> €/$0 per month for 2.5GB of Data Transfer and 5GB of Database Storage.  
> €/$1 for every additional 100MB of Data Transfer.  
> €/$3 per month for every additional 5GB of Database Storage <br/><br/>
> Data Transfer means the movement of all data to or from the HERE XYZ service.  
> Database Storage means data stored by the HERE XYZ service.  
> Learn more at developer.here.com/plans  


🔨 **IMPROVED** 🔧

- There is now an option in published projects allowing anyone to provide us with feedback or report an issue.
- Improved error messaging for file uploads.
- We’ve removed shapefile support from Studio file uploading, but added link to a tutorial for using the HERE CLI to upload shapefiles. The CLI can provide a more comprehensive approach for handling the unique requirements of shape files.

🐜 **FIXED** 🐜

- Fixed a bug where properties named “id” could not be used to create style groups.

🔮 **COMING SOON** 🔮

- Improved map performance for large Spaces.

---

### Studio release 0.2.0

This one’s all about the tables! We’ve made improvements to the functionality and performance of our data tables in Studio. 

> TL;DR: While editing a project, the features you see on the map will be the features you see in the table view. Improved performance for inspecting large Spaces in Data Hub.

> Coming soon: account dashboard and improved map performance.

:sparkles: **NEW** :sparkles:

We’ve strengthened the connection between what you see on the map and what you see on the data table. When you are editing a project, the features that are currently visible on the map will be the features available on the table. This will help you narrow down the features you are working with on the table, and will also improve Studio performance.

:hammer: **IMPROVED** :wrench:

We’ve also improved functionality for inspecting your Spaces via the Studio Data Hub.
For Spaces with a smaller number of features, you will continue to be able to search, add columns, delete rows and edit cells.
For Spaces with a large number of features, we’ve introduced pagination and restricted some functionality, which improves the table’s overall performance.

:ant: **FIXED** :ant:

Feature property values are now being saved as the correct type (for example, as a number if it is a number) after edits and for new additions (new columns and new features).

:crystal_ball: **COMING SOON** :crystal_ball:

Account dashboard: a place to manage your plan(s) and check in on your usage.
Improved map performance for large Spaces.

---
  
### Studio and Viewer release 0.1.18

Today we’re excited to introduce a new experience for mobile users and updated basemaps!

> TL;DR: A new UX for mobile devices, Improved mobile UX for published projects, Updated basemaps, Studio dashboard view preference is now saved.

> Coming soon: improved data loading, account management dashboard

:sparkles: **NEW** :sparkles:

Now you can manage your projects on the go! We’re introducing a new Studio experience for mobile devices. You can view your project dashboard, adjust project settings, and easily view your published maps.

:hammer: **IMPROVED** :wrench:

We’ve also updated the mobile experience for published maps, making it easier to view feature cards alongside the legend panel that was introduced in our last release. 

Our basemap styles have received a polish, and they’re looking spiffy. You’ll see the familiar color themes, but notice enhanced contrast for data visualization, as well as more detailed line work and road hierarchy across zooms. 

As requested, your preference for list view or thumbnail view on the Studio dashboard is now saved.

:ant: **FIXED** :ant:

A few fixes were made for feature editing in Studio.

:crystal_ball: **COMING SOON** :crystal_ball:

Improved data loading, for a smoother experience when working with large datasets.
Account management dashboard, for checking up on your data usage.

---

### Studio and Viewer release 0.1.17
The XYZ Studio team is back with some new features that will help you tell even more compelling stories with your maps!

> TL;DR: You can now add a legend to your published projects! You can also add a title and description. There is a new, easy way to turn cards on or off for published projects.

> Coming soon: improved experience on mobile devices.

:sparkles: **NEW** :sparkles:

At long last, legends! You can now add a legend to your published project, which will show the symbol and name for each style group on your map.  Preview your legend in Publish Settings, and if you like what you see, you can add it right then and there to your project. We hope you’re as excited about this as we are.

_But wait, there’s more_: In Publish Settings, you can also choose to add your project’s title and description to the published version. You can now easily turn off cards completely for your published project. If the Cards option is not selected in Publish Settings, feature information cards will not be shown at all in your published project. If you select the Cards option, they will show on click, with an option to view on hover, in your published project.
You probably did not need that explanation of how checkboxes work.

:hammer: **IMPROVED** :wrench:

You can now create style groups based on `>= (greater than or equal to)` or `<= (less than or equal to)` conditions. This improvement is guaranteed to make your new style groups >=  all your previous styles. Style group symbols in the Studio side panel now more closely match the symbols on your map, rather than simply indicating feature type and color. Fancy!

:ant: **FIXED** :ant:

An error that would occasionally be shown during the file upload process, despite a successful upload, has been fixed.
Style groups based on Feature ID are now rendering correctly on published maps. Other minor bug fixes.


:crystal_ball: **COMING SOON** :crystal_ball:

In our next release, we’ll be introducing an improved mobile experience. You’ll be able to log into Studio, view your projects, and access Publish Settings for your projects.

---

### Studio and Viewer release 0.1.16
We are ringing in the new year with some exciting new features in Studio! We’re also trying out a new format for release notes, with the hope that it will be easier to understand the improvements and fixes in each release, and how they impact your use of Studio. Let us know what you think!

> TL;DR: Create style groups to style data according to property values. Drag and drop a file to the map to begin uploading the file for use in that project.

> Coming soon: options to display the project title, description, and legend in published maps.

:sparkles: **NEW** :sparkles:

You now have the ability to style the data on your map according to feature property values!

Where you once could adjust the style of the features on your map as a single group, there is now an option to add a new point/line/polygon style. This button opens a popup where you can define a style group.

You can also create a new group by clicking on a feature on the map, and then clicking the paint roller icon next to a property value in the sidebar. This will create a new group based on that property value, and you can always customize it further.

:world_map: *Example:* Say you are working with a building dataset, and there is a `“year_built”` property. You could create a style group for features where `“year_built”`  is greater than 1990. All features in the dataset where the building was built after 1990 would be included in the style group, and you can easily highlight these features on your map.

You can add on to rules using the “and” button to further filter the features included in the group.

:world_map: *Example:* Using the building dataset example from above, let’s say there is also a property called `“frame_material”`. You could also click the “and” button to add `“frame_material equals wood”`, to include only wood-frame buildings built after 1990 in the group.

To adjust or delete a group, click the gear icon next to the group name in the feature styling interface.

When you have multiple style groups within a feature type (points, lines or polygons), you can adjust the order of appearance on the map by dragging the rule group up or down in the styling interface.

You can now drag and drop a file to the map in a project to begin uploading the file for use in that (and other) projects.

Dragging a dropping a file to the map will open the “Add data” interface, and once your file is done uploading, it will be auto-selected to add to your project— you just need to click the “Add 1 dataset” button.

:crystal_ball: **COMING SOON** :crystal_ball:

We will soon be releasing new functionality that will provide the option to display a project’s title, description, and legend information in the published map. It will be a great way to help you tell a story through your map, and will show off all the cool new style groups you’ve created.

---
