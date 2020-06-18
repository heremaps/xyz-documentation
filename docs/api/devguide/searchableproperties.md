# Adjust searchable feature-properties

!!! Note "Your account needs access to the Data Hub Add-on Services."

This section describes how to use the extended capability of enabling property-search for
user-specified properties of your GeoJSON features inside a space. 

It is recommended that you read the ["properties-search"](propertiessearch.md) guide before going on
with this more specific guide.

## Searching for features in a space

If the above prerequisites are fulfilled, adjusting the searchable properties can be done by
updating the space's `searchableProperties` property. This can be done using a `PATCH` request to
the `/spaces/{spaceId}` endpoint.

`searchableProperties` is a map of which the keys are property names and the values are boolean
flags telling whether the property should be searchable or not.

The following sample shows how to define `someProperty1` to be searchable and `someProperty2` to
be not searchable. In case you're wondering about the latter: That could be necessary to tell Data Hub
to revoke the decision of making `someProperty2` searchable in the automated algorithm.

**TL;DR**
*Data Hub has a space-specific algorithm to automatically decide which of the space's properties
are searchable. In case you desire other properties to be searchable the `searchableProperties` map
can be used to define that.*

```JSON
{
  "someProperty1": true,
  "someProperty2": false
}
```
Nested properties can be specified using the dot-notation e.g.:

```JSON
{
  "some.nested.property": true
}
```

*Remember: In a `PATCH`-operation only the properties to be changed are necessary*

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Spaces/patchSpace)*

```HTTP
PATCH /spaces/{spaceId}
```

with the following body

```JSON
{
    "searchableProperties": {
        "someProperty1": true,
        "someProperty2": false,
        "some.nested.property": true
    }
}
```

### Response

```JSON
{
    "title": "My Demo Space",
    "description": "Description as markdown",
    "searchableProperties": {
        "someProperty1": true,
        "someProperty2": false,
        "some.nested.property": true
    }
}
```
