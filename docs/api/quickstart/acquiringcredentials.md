# Acquiring Credentials

All users must use tokens to access the HERE XYZ Hub. 

In order to generate tokens, first visit http://xyz.here.com/studio and sign up for an account. Once you have an account, you can sign into the [Token Manager](https://xyz.api.here.com/token-ui/) to generate and manage tokens.  

The bearer token is provided as a value of the “Authorization” header parameter using the “Bearer” authentication scheme:

```HTTP
Authorization: Bearer <token>
```

or alternatively, as a URI query parameter:

```HTTP
access_token=<token>
```

You can read more about how to use tokens on the [Generate tokens](../getting-token.md) page.
