# Acquiring Credentials

All users of the HERE XYZ Hub must get authentication and authorization credentials. These credentials are assigned per application, and a bearer token is obtained by using the app_id and app_code issued by the HERE Developer Portal

The bearer token is provided as a value of the “Authorization” header parameter using the “Bearer” authentication scheme:

```HTTP
Authorization: Bearer <token>
```

or alternatively, as a URI query parameter:

```HTTP
access_token=<token>
```

To obtain the credentials for an application, please visit <http://developer.here.com/plans> to register with HERE.

You can read more about tokens, where to get and how to use them on the [Generate tokens](../getting-token.md) page.