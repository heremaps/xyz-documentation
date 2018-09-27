# Reading Spaces

Once you have acquired your credentials. you can find out which spaces you have access to.

## Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Read_Spaces)*

```HTTP
GET /spaces
```

## Response

```JSON
[
    {
        "id": "x-demospace",
        "title": "My Demo Space",
        "description": "Description as markdown",
    }
]
```