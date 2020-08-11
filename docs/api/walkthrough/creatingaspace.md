# Create a Space

> #### Note
>
> The endpoint for the API is <https://xyz.api.here.com/hub>.

## Request

*Try in [Swagger](https://xyz.api.here.com/hub/static/swagger/#/Edit_Spaces)*

```HTTP
POST /spaces
```

```JSON
{

    "title": "My Demo Space",
    "description": "Description as markdown"

}
```

## Response

```JSON
{
    "id": "x-demospace",
    "owner":"{appId}",
    "title": "My Demo Space",
    "description": "Description as markdown"
}
```
