# Describing Forms and Multi-part Content

In order to describe the structure of HTML Forms and multipart media types, OAS enables repurposing of the Schema Object to describe the structure.  The key-value pairs of a `application/x-www-form-urlencoded` are modelled as the properties of an `object`.

```yaml
openapi: 3.0.0
info:
  title: Example of HTML forms
  version: 1.0.0
paths:
  '/submit':
    post:
      requestBody:
        content:
          application/x-www-form-urlencoded:
            schema:
              type: object
              properties:
                street:
                  type: string
                city:
                  type: string
                province:
                  type: string
                country:
                  type: string
                postcode:
                  type: string
                tags:
                  type: array
                  items:
                    type: string
      responses:
        '200':
          description: Great Success!
```

## Multipart content

For multipart content use of the Schema Object is stretch a little further and is used to describe the structure of the various parts. Where this is not possible the encoding object is introduced to map the parts of the payload to a media type.  

```yaml
openapi: 3.0.0
info:
  title: Example of multi-part form
  version: 1.0.0
paths:
  '/submit':
    post:
      requestBody:
        content:
          multipart/mixed:
            schema:
              type: object
              properties:
                id:
                  # default is text/plain
                  type: string
                  format: uuid
                address:
                  # default is application/json
                  type: object
                  properties: {}
                historyMetadata:
                  # need to declare XML format!
                  description: metadata in XML format
                  type: object
                  properties: {}
                profileImage:
                  # default is application/octet-stream, need to declare an image type only!
                  type: string
                  format: binary
            encoding:
              historyMetadata:
                # require XML Content-Type in utf-8 encoding
                contentType: application/xml; charset=utf-8
              profileImage:
                # only accept png/jpeg
                contentType: image/png, image/jpeg
      responses:
        '200':
          description: Success!
```

The encoding object also enables declaring HTTP headers that are associated to the part, and can even affect serialization of `application/x-www-form-urlencoded` parameters.
