# JSON Schema

JSON Schema provides vocabulary to define application-specific JSON documents. The authoritative resource for this topic is [json-schema.org]("https://json-schema.org/").

## Most basic Schema

The most basic schema is  

```JSON
{ }
```

an empty JSON Object or (since draft 6 of the specification)

```JSON
true
```

effectively accepting any valid JSON.
To reject everything with setting

```JSON
false
```

as the schema.

### Declaring a Schema with $schema

It is recommended to declare that a JSON fragment is a JSON Schema by using the **$schema** keyword in the root of the schema.

```JSON
{ "$schema": "http://json-schema.org/schema#" }
```

If your schema was written against a specific version, you should include the draft name in the path, for example:

```JSON
{ "$schema": "http://json-schema.org/draft-04/schema#" }
```

### Identify a Schema with $id

It is best practice to include a unique identifier for each schema

```JSON
{ "$id": "http://yourdomain.com/schemas/myschema.json" }
```

!!! Warning "HERE XYZ does not load external schema resources"

**$id** can also be used to reference a subschema without using JSON Pointer.

```JSON
{
  "$schema": "http://json-schema.org/draft-07/schema#",

  "definitions": {
    "address": {
      "$id": "#address",
      "type": "object",
      "properties": {
        "street_address": { "type": "string" },
        "city":           { "type": "string" },
        "state":          { "type": "string" }
      },
      "required": ["street_address", "city", "state"]
    }
  },

  "type": "object",

  "properties": {
    "billing_address": { "$ref": "#address" },
    "shipping_address": { "$ref": "#address" }
  }
}
```

!!! Warning "The $id properties of the schema must not include whitespace characters"

## Primitive Types

A JSON document to which a schema is applied is known as an 'instance'. An instance has one of six primitive types and a range of possible values:

1. *object*  
: An unordered set of properties mapping a string to an instance
2. *array*  
: an **ordered** list of instances
3. *string*  
: A string of Unicode characters
4. *number*  
: An arbitrary-precision base-10 decimal number value
5. *boolean*  
: A "true" or "false" value</dd>
6. *null*  
: JSON null

[Understanding JSON Schema](https://json-schema.org/understanding-json-schema/) provides a good overview of JSON Schema and the details of subtypes of the above.

## Example JSON Schema for GeoJSON

```JSON
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "type": {
      "type": "string"
    },
    "geometry": {
      "oneof":[
        {
          "type": "null"
        },{
          "type":"object"
        }
      ]
    },
    "properties": {
      "type": "object",
      "properties": {
        "name": {
          "type": "string"
        }
      },
      "required": [
        "name"
      ]
    }
  },
  "required": [
    "type",
    "geometry",
    "properties"
  ]
}
```
