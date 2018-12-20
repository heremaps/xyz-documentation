XYZ Hub is a real-time cloud-based location hub for discovering, storing, retrieving, manipulating and publishing private or public mapping data.

It uses the concepts of **Spaces** to store your data. A Space is your own geospatial
data repository, which you can quickly create when needed to store data.

To interact with the HERE XYZ Hub API directly from your application you just need to interact with our public REST APIs. They are simple and straightforward to use from any application environment - you only need to know how to make RESTful requests.

## Authentication

For everything you want to do via the API, you need to use a token as described in the
[Generate Token Section](/api/getting-token.md).

[![API](../assets/images/api-auth.png)](../assets/images/api-auth.png)


## Data

When you have a space it's very easy to store any GeoJSON data you want in a Space and retrieve it when needed.
No need to worry about how to store data effectively if you want to use it on a map - in fact
you don't have to understand the geospatial details. We make sure that we put your content in a
safe place (the space) so it can be used very efficiently on a map.

Spaces are by definition worldwide and can include any type of GeoJSON feature. Within
each feature, you can have different `properties` as the payload information.

Every feature is identified by its `id` which is unique in the Space. You can read or update
features directly using this identifier easily. And if you don't provide it yourself, the
API will go ahead and generate it for you - one less thing to worry about.

## Tags

To work efficiently with data, it is often necessary or helpful to sub-divide a data. Spaces
allow you to do this with the help of `tags`. A Tag is just a text that can be used to only
concern yourself with a subset of the data. They are completely up to you (or the owner of the
Space you are working with).

So you might want to use different tags for the different ways you want to use or style data.
For example, you might want to tag the Store location you are putting in a layer with the
amenities that the particular store provides (`coffee`, `food`, `late_night`). In addition
you could use `new` as a tag for newly opened stores you want to highlight and call out on
the map.

## Iterating

Another core concept when you are working Space it to [iterate](https://xyz.api.here.com/hub/static/redoc/#operation/iterateFeatures) over the content to get everything that you are interested in.

As you might access with *a lot* of data, you will retrieve it in parts. With the `limit` parameter
you tell the API how much data you want to see at once, while the `handle` is just something the
API gives you together with the data, that you need to provide back when you want to continue to
get the next chunk of data.

## ReDoc API documentation
[`https://xyz.api.here.com/hub/static/redoc/`](https://xyz.api.here.com/hub/static/redoc/)


## Open API documentation
[`https://xyz.api.here.com/hub/static/swagger/`](https://xyz.api.here.com/hub/static/swagger/)
