# Path Descriptions

In OpenAPI v2 the focus was on documenting the individual operations.  However, some users prefer to document their APIs from a resource perspective and only provide additional explanation when the usage of HTTP methods vary from the default.

```yaml
openapi: 3.0.0
info:
  title: Example of documenting paths instead of operations
  version: 1.0.0
paths:
  '/speakers':
    summary: A set of speakers
    description: |- 
      This resource is a set of speakers presenting at a conference.  Speakers information can be represented in either `text/plain`, or `application/vnd.collection+json`
    get:
      responses:
        '200':
          description: List of speakers
          content: 
            text/plain: {}
            application/vnd.collection+json: {}
    post:
      description: |- 
        Use a `application/x-www-urlencoded-form` to send speaker related information
      responses:
        '201':
          description: Acknowledge creation of a new speaker resource
          headers:
            Location:
              schema: 
                type: string
                format: uri
    delete:
      responses:
        '204':
          description: Acknowledge successful deleted
          
```


