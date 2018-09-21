# File uploads and downloads

In OpenAPI V2 the Schema Object supported schemas with `type` equal to `file`.  This type is no longer supported as it is not an allowed type in JSON Schema.

## Motivation

Numerous users of OpenAPI have expressed their desire to use standard JSON Schema libraries to do some validation based on schemas in OpenAPI documents.  The existence of the `file` type caused standard JSON Schema validation libraries to fail.  Now with the removal of this, it should no longer be a problem.

## Describing a file payload in OpenAPI v3

```yaml
openapi: 3.0.0
info:
  title: API to upload a file
  version: 1.0.0
paths:
  /files/{filename}:
    put:
      parameters:
        - name: filename
          in: path
          required: true
          schema:
            type: string
      requestBody:
        content:
          application/octet-stream:
            schema:
              type: string
              format: binary
      responses:
        default:
          description: OK
``` 

Using the `type` as `string` and `format` as `binary` is the functional equivalent of the `file` type. 
