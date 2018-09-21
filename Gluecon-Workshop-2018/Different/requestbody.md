# Request Body

In OpenAPI V2, request bodies were defined as a type of parameter. In V3 they are a distinct object with their own set of rules.

## Motivation

Although in V2 request bodies were a parameter they had their own distinct set of rules:
- could have a schema
- name property wasn't used
- Media types of the request body parameter defined by a consumes that was either specified at the operation level or globally.

Request bodies that were `application/x-www-form-urlencoded` were handled inconsistently than other media types.

The Request Body Object models the fact that HTTP treats request bodies significantly differently than URI parameters and header values.

## OpenAPI V2

```yaml
swagger: '2.0'
info:
  title: Example of request body in 2.0
  version: 1.0.0

paths:
  /opinions:
    post:
      consumes:
       - text/plain
      parameters:
        - name: AnOpinion
          in: body
          schema:
            type: string
            example: I think the V3 way is cleaner
      responses:
        '200':
          description: OK
```
In OpenAPI V3, each operation that is allowed to have a body, can include a `requestBody` object which contains a `content` object that describes the details of the payload.  The same `content` object is used to describe `responses` which makes the description of payloads more consistent.

```yaml
openapi: 3.0.0
info:
  title: Example of request body in 3.0
  version: 1.0.0
paths:
  /opinions:
    post:
      requestBody:
        description: ''
        required: true
        content:
          text/plain:
            schema:
              type: string
            example:
              I think the V3 way is cleaner
          application/json:
            schema:
              type: string
      responses:
        '200':
          description: OK
```

