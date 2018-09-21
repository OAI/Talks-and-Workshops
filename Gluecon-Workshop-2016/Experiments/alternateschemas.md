# Alternate Schemas

### Native XML Schema
```yaml
openapi: 3.0.0
info:
  title: Examples of alternative schemas
  version: 1.0.0

paths:
  '/user/{username}'
    name: username
    in: path
    description: username to fetch,
    required: true
responses:
  '200':
    description: An XML representation of a user
    content: 
      application/xml: 
        schematype: XSD
        schemaRef : schema/username.xsd 
```

### ABNF Schema for headers
``` yaml
openapi: 3.0.0
info:
  title: Examples of alternative schemas
  version: 1.0.0
paths: {}
components:
  headers: 
    my-user-agent:
      content:
        text/plain:  
          schematype: HTTP-ABNF
          schema: product *( RWS ( product / comment ) )
          example : foo/v1.0 (Yo! like my app?) OS/2.0
```