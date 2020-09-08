# Use Schema Validation

> #### Note
>
> Your account needs access to the Data Hub Add-on Services.

To use Schema Validation you need to put additional data into the space
definition. Please add a processor with the ID *schema-validator* and put
either a URL to a valid JSON schema or the complete schema as JSON string into
the param *schema*:

```json5
{
    "title": "My Space with JSON Schema Validation",
    "description": "Description as markdown",
    "processors": [
        {
            "id": "schema-validator",
            "params": {
                "schema": "<URL to schema or schema as JSON schema>"
            }
        }
    ]
}
```

> #### Note
>
> If you use an URL please make sure that the URL is public accessible.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Spaces/postSpace)*

```HTTP
POST /spaces
```

```json5
{
    "title": "My Space with JSON Schema Validation",
    "description": "Make sure that all features contain a field 'geometry' and a property 'name'.",
    "processors": [
        {
            "id": "schema-validator",
            "params": {
                "schema": "{\"definitions\":{},\"$schema\":\"http://json-schema.org/draft-07/schema#\",\"$id\":\"http://example.com/root.json\",\"type\":\"object\",\"title\":\"TheRootSchema\",\"required\":[\"geometry\",\"type\",\"properties\"],\"properties\":{\"geometry\":{\"$id\":\"#/properties/geometry\",\"type\":\"object\",\"title\":\"TheGeometrySchema\",\"required\":[\"type\",\"coordinates\"],\"properties\":{\"type\":{\"$id\":\"#/properties/geometry/properties/type\",\"type\":\"string\",\"title\":\"TheTypeSchema\",\"default\":\"\",\"examples\":[\"Point\"],\"pattern\":\"^(.*)$\"},\"coordinates\":{\"$id\":\"#/properties/geometry/properties/coordinates\",\"type\":\"array\",\"title\":\"TheCoordinatesSchema\",\"items\":{\"$id\":\"#/properties/geometry/properties/coordinates/items\",\"type\":\"number\",\"title\":\"TheItemsSchema\",\"default\":0.0,\"examples\":[14.3222,-2.32506]}}}},\"type\":{\"$id\":\"#/properties/type\",\"type\":\"string\",\"title\":\"TheTypeSchema\",\"default\":\"\",\"examples\":[\"Feature\"],\"pattern\":\"^(.*)$\"},\"properties\":{\"$id\":\"#/properties/properties\",\"type\":\"object\",\"title\":\"ThePropertiesSchema\",\"required\":[\"name\",\"@ns:com:here:xyz\"],\"properties\":{\"name\":{\"$id\":\"#/properties/properties/properties/name\",\"type\":\"string\",\"title\":\"TheNameSchema\",\"default\":\"\",\"examples\":[\"Toyota\"],\"pattern\":\"^(.*)$\"},\"@ns:com:here:xyz\":{\"$id\":\"#/properties/properties/properties/@ns:com:here:xyz\",\"type\":\"object\",\"title\":\"The@ns:com:here:xyzSchema\",\"required\":[\"tags\"],\"properties\":{\"tags\":{\"$id\":\"#/properties/properties/properties/@ns:com:here:xyz/properties/tags\",\"type\":\"array\",\"title\":\"TheTagsSchema\",\"items\":{\"$id\":\"#/properties/properties/properties/@ns:com:here:xyz/properties/tags/items\",\"type\":\"string\",\"title\":\"TheItemsSchema\",\"default\":\"\",\"examples\":[\"yellow\"],\"pattern\":\"^(.*)$\"}}}}}}}}"
            }
        }
    ]
}
```

> #### Warning
>
> The $id properties of the schema must not include whitespace characters.

### Response

```json5
{
    "id": "x-demospace",
    "owner":"{appId}",
    "title": "My Space with JSON Schema Validation",
    "description": "Make sure that all features contain a field 'geometry' and a property 'name'.",
    "processors": [
        {
            "id": "schema-validator",
            "params": {
                 "schemaUrl": "<Location of schema in the Data Hub managed S3 bucket>"
             }
        }
    ]
    //,...
}
```

Please note that the field *schema* has been replaced with a field *schemaUrl*
that points to a private copy in the Data Hub S3 bucket.

## Validating your data

Just POST/PUT your data like you would without Schema Validation. If the
features are valid they will be stored. All failing objects will **NOT** be
stored and position, ID (if set by the developer) and error messages will be
returned. Please note that the position is zero based, so 0 is the first object,
1 the second and so on.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Features/putFeatures)*

```HTTP
PUT /spaces/{spaceId}/features
```

with the corresponding body:

### Request body

```json5
{
    "type": "FeatureCollection",
    "features": [
        {
            "geometry": {
                "type": "Point",
                "coordinates": [
                    14.3222,
                    -2.32506
                ]
            },
            "type": "Feature",
            // typo in properties
            "propertiess": {
                "name": "Toyota"
            }
        },
        {
            "geometry": {
                "type": "Point",
                "coordinates": [
                    15.8319,
                    -2.5913
                ]
            },
            "type": "Feature",
            "properties": {
                "name": "Audi"
            }
        },
        {
            // Object is missing geometry
            "type": "Feature",
            "properties": {
                "name": "Tesla"
            }
        }
    ]
}
```

### Response

```json5
{
    "type": "FeatureCollection",
    "inserted": [
        "dxAH19uIUDCU7kch"
    ],
    "etag": "016524948bfc1213",
    "streamId": "5fb211eb-973f-11e9-9406-258558785f8d",
    "failed": [
        {
            "id": null,
            "position": 0,
            "message": "Feature on position 0 has JSON schema validations errors/warnings.\n[[1,184][/properties] The object must have a property whose name is \"name\".]"
        },
        {
            "id": null,
            "position": 2,
            "message": "Feature on position 2 has JSON schema validations errors/warnings.\n[[1,107][] The object must have a property whose name is \"geometry\".]"
        }
    ],
    "features": [
        {
            "type": "Feature",
            "properties": {
                "name": "Audi",
                "@ns:com:here:xyz": {
                    "space": "X0Bphg0Q",
                    "tags": [],
                    "createdAt": 1561463405914,
                    "updatedAt": 1561463405914
                }
            },
            "bbox": [
                15.8319,
                -2.5913,
                15.8319,
                -2.5913
            ],
            "id": "dxAH19uIUDCU7kch",
            "geometry": {
                "type": "Point",
                "coordinates": [
                    15.8319,
                    -2.5913
                ]
            }
        }
    ]
}
```

As only the Audi feature was valid, it is the only one stored. The other two
features failed because the first (position 0) has a typo in *properties* and so
it missing the *properties.name* field. The last feature (position 2) is missing
a *geometry* field.

## Upload a new schema

To use a different JSON schema you need to update the space definition with the
new schema JSON string or URL.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Spaces/patchSpace)*

```HTTP
PATCH /spaces/{spaceId}
```

```json5
{
    "title": "My Space with JSON Schema Validation",
    "description": "Make sure that all features contain a field 'geometry' and a property 'name'.",
    "processors": [
        {
            "id": "schema-validator",
            "params": {
                "schema": "{\"definitions\":{},\"$schema\":\"http://json-schema.org/draft-07/schema#\",\"$id\":\"http://example.com/root.json\",\"type\":\"object\",\"title\":\"TheRootSchema\",\"required\":[\"geometry\",\"type\",\"properties\"],\"properties\":{\"geometry\":{\"$id\":\"#/properties/geometry\",\"type\":\"object\",\"title\":\"TheGeometrySchema\",\"required\":[\"type\",\"coordinates\"],\"properties\":{\"type\":{\"$id\":\"#/properties/geometry/properties/type\",\"type\":\"string\",\"title\":\"TheTypeSchema\",\"default\":\"\",\"examples\":[\"Point\"],\"pattern\":\"^(.*)$\"},\"coordinates\":{\"$id\":\"#/properties/geometry/properties/coordinates\",\"type\":\"array\",\"title\":\"TheCoordinatesSchema\",\"items\":{\"$id\":\"#/properties/geometry/properties/coordinates/items\",\"type\":\"number\",\"title\":\"TheItemsSchema\",\"default\":0.0,\"examples\":[14.3222,-2.32506]}}}},\"type\":{\"$id\":\"#/properties/type\",\"type\":\"string\",\"title\":\"TheTypeSchema\",\"default\":\"\",\"examples\":[\"Feature\"],\"pattern\":\"^(.*)$\"},\"properties\":{\"$id\":\"#/properties/properties\",\"type\":\"object\",\"title\":\"ThePropertiesSchema\",\"required\":[\"name\",\"@ns:com:here:xyz\"],\"properties\":{\"name\":{\"$id\":\"#/properties/properties/properties/name\",\"type\":\"string\",\"title\":\"TheNameSchema\",\"default\":\"\",\"examples\":[\"Toyota\"],\"pattern\":\"^(.*)$\"},\"@ns:com:here:xyz\":{\"$id\":\"#/properties/properties/properties/@ns:com:here:xyz\",\"type\":\"object\",\"title\":\"The@ns:com:here:xyzSchema\",\"required\":[\"tags\"],\"properties\":{\"tags\":{\"$id\":\"#/properties/properties/properties/@ns:com:here:xyz/properties/tags\",\"type\":\"array\",\"title\":\"TheTagsSchema\",\"items\":{\"$id\":\"#/properties/properties/properties/@ns:com:here:xyz/properties/tags/items\",\"type\":\"string\",\"title\":\"TheItemsSchema\",\"default\":\"\",\"examples\":[\"yellow\"],\"pattern\":\"^(.*)$\"}}}}}}}}"
            }
        }
    ]
}
```

### Response

```json5
{
    "id": "x-demospace",
    "owner":"{appId}",
    "title": "My Space with JSON Schema Validation",
    "description": "Make sure that all features contain a field 'geometry' and a property 'name'.",
    "processors": [
        {
            "id": "schema-validator",
            "params": {
                 "schemaUrl": "<Location of updated schema in the Data Hub managed S3 bucket>"
             }
        }

    ]
    //,...
}
```

## Disabling Schema Validation

To disable the Schema Validation just update the space definition but not send
the *schema-validator* processor definition. Please note that you have to
include **all other** processor definitions if there are multiple; otherwise
you will disable all processors.

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit%20Spaces/patchSpace)*

```HTTP
PATCH /spaces/{spaceId}
```

```json5
{
    "title": "My Space with JSON Schema Validation",
    "description": "Make sure that all features contain a field 'geometry' and a property 'name'.",
    "processors": []
}
```

### Response

```json5
{
    "id": "x-demospace",
    "owner":"{appId}",
    "title": "My Space with JSON Schema Validation",
    "description": "Make sure that all features contain a field 'geometry' and a property 'name'.",
    "processors": [],
    //,...
}
```
