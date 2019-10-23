# Working with Spaces

## Creating a Space

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Spaces)*

```HTTP
POST /spaces
```

As it is a POST request, it has to have at least the following body.

```JSON
{
    "title": "My Demo Space",
    "description": "A description which may contain ***markdown*** syntax"
}
```

### Response

```JSON
{
    "id": "x-demospace",
    "title": "My Demo Space",
    "description": "A description which may contain ***markdown*** syntax"
}
```

## Reading a Specific Space

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Spaces)*

```HTTP
GET /spaces/{spaceId}
```

### Response

```JSON
{
  "id": "{spaceId}",
  "title": "My Demo Space",
  "description": "A description which may contain ***markdown*** syntax"
}
```

## Reading all Spaces

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Spaces)*

```HTTP
GET /spaces
```

### Response

```JSON
[
    {
        "id": "x-demospace",
        "title": "My Demo Space",
        "description": "A description which may contain ***markdown*** syntax"
    },
    {
        "id": "x-trees",
        "title": "A public space",
        "description": "All the old oaks in Berlin" 
    }
]
```

## Updating a Space

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Spaces)*

```HTTP
PUT /spaces/{spaceId}
```

which requires a body like the following:

```JSON
{
    "title": "My Demo Space",
    "description": "**Altered** Description"
}
```

### Response

```JSON
{
    "title": "My Demo Space",
    "description": "**Altered** Description"
}
```

## Deleting a Space

### Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Spaces)*

```HTTP
DELETE /spaces/{spaceId}
```

A successful response to this request is the following

### Response

```HTTP
HTTP/1.1 204 No Content
```