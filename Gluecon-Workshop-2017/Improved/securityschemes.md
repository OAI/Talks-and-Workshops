# Security Schemes
The security schemes has been slightly improved to provide a more general solution to HTTP authorization and provide more flexibility when it comes to OAuth2 based security.

## HTTP Authorization
Where OpenAPI V2 had a security type called `basic`, that has now been replaced with a type called `http` and a property called `scheme` that identifies the type of HTTP Authorization.  This enables support for a wider range of security mechanisms.

```yaml
openapi: 3.0.0
info:
  title: All kinds of security options using the Authorization header
  version: 1.0.0
components:
  securitySchemes:
    basic:
      type: http
      scheme: basic
    bearer:
      type: http
      scheme: bearer
      bearerFormat: JWT
    oauth1:
      type: http
      scheme: oauth
    digest:
      type: http
      scheme: digest
security:
- basic: []
- bearer : []
- oauth1 : []
- digest : []
paths: {}
```

The `bearerFormat` property is only valid for scheme `bearer` and is primarily just used for documentation purposes and a client application should not be required to understand the internal structure of a bearer token.

## OAuth2 Enhancements

The updated OAuth2 mechanism now allows describing multiple flows and also provides support for discovering where to refresh tokens.  Also, where OAuth2 is described by an OpenID Connect discovery mechanism, a link can now be provided as an alternative to describing all the metadata inline.

```yaml
openapi: 3.0.0
info:
  title: OAuth2 with multiple flows
  version: 1.0.0
components:
  securitySchemes:
    myOauth2:
      type: oauth2
      flows: 
        implicit:
          authorizationUrl: https://example.com/api/oauth/dialog
          scopes:
            write:pets: modify pets in your account
            read:pets: read your pets
        authorizationCode:
          authorizationUrl: https://example.com/api/oauth/dialog
          tokenUrl: https://example.com/api/oauth/token
          refreshUrl: https://example.com/api/oauth/refresh
          scopes:
            write:pets: modify pets in your account
            read:pets: read your pets
    myOpenIdConnect:
      type: openIdConnect
      openIdConnectUrl: https://example.com/.well-known/openid-connect
security:
- myOauth2: []
- myOpenIdConnect: []
paths: {}
```
