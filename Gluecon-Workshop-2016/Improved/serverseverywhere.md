# Servers Everywhere

OpenAPI V3 is not limited to describing resources on a single server.  By overriding servers on a path or operation, you can bring resources from different servers.

```yaml
openapi: 3.0.0
info:
  title: Servers Everywhere
  version: 1.0.0
servers:
- url: https://api.example.com
paths:
  '/passports/{id}':
    get:
      responses:
        '200': 
          description: Ok
    post:
      servers:                    # Write operation on a different server
      - url: https://supersecure-api.example.com
      requestBody:
        content:
          application/json: {}
      responses:
        '200': 
          description: Ok
  '/images/{filename}':
    summary: Pictures of people
    servers:                      # Static resources on a different server
    - url: https://static.example.com
    get: 
      responses:
        '200':
          description: Ok
    put:
      responses:
        '200':
          description: Ok
    delete:  
      responses:
        '200':
          description: Ok
```
