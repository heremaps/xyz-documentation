# Common Errors

This page contains common error messages encountered when using the API.

## Schema Validation

### The value must be a valid URI reference.

If the schema property "$id" contains one or more whitespace characters, the
schema validation will report the error like "The value must be a valid URI
reference".

Please update your schema and make sure that all "$id" values do not contain
whitespace characters.

### The object must have a property whose name is "@ns:com:here:xyz".

During validation we will remove the field "@ns:com:here:xyz" containing
tags, space ID, and a few other values from the properties. This is done
to ensure that the JSON schema only checks user and not system generates
properties.

This means that JSON Schema used for validation must not contain
"@ns:com.here.xyz" as a required property, because this property and its
children will be removed before validation and only re-added afterwards.

### The schema reference "/myschemafile.json"("http://mydomain/myschemafile.json") cannot be resolved.

We will not load resolve external schema definitions, even if the URL points
to a valid (sub-)schema. Please use the *definitions* keyword to merge multiple
schema files into one.
