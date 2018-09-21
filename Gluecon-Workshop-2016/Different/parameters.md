# Parameters

In V3 parameters are still an array and must have a unique combination of `name` and `in` value.  However, `in` can no longer have the value `body` or `form`, but it can now have `cookie`.
Instead of using a subset of JSON schema properties to describe the parameter value, an OpenAPI Schema Object can be included in the parameter definition.

## Simple Parameters in V3

All parameters can now have schemas.  
```yaml
openapi: 3.0.0
info:
  title: The simplest parameter example
  version: 1.0.0
paths:
  /customer/{id}:    # https://api.example.org/customer/72
    parameters:
    - name: id
      in: path
      required: true
      schema:
        type: integer
        title: A unique identifier for the customer
```

There is no more `collectionFormat`.  Parameters that contain multiple values are handled by the new `style` property.

```yaml
openapi: 3.0.0
info:
  title: A parameter with an array of ints
  version: 1.0.0
paths:
  /customers:      # https://api.example.org/customers?ids=34,45,67
    parameters:
    - name: ids
      in: query
      style: simple
      schema:
        type: array
        items:
          type: integer
```

The `style` parameter supports a number of URI formats described by the URI Template spec [rfc6570](https://tools.ietf.org/html/rfc6570).

```yaml
openapi: 3.0.0
info:
  title: A version and revision matrix parameter
  version: 1.0.0
paths:
  /{version}{rev}/customers:      # https://api.example.org/v2;rev=2/customers
    parameters:
    - name: version
      in: path
      required: true
      schema: 
        type: string
    - name: rev
      in: path
      required: true
      style: matrix
      schema:
        type: integer
```

By passing a parameter that is a hash of values, query parameters can be described on the dynamically. 

```yaml
openapi: 3.0.0
info:
  title: A map of parameter values
  version: 1.0.0
paths:
  /customers:      # https://api.example.org/customers?active=true&country=Canada&category=first
    parameters:
    - name: filters
      in: query
      style: form
      schema: 
        type: object
```

## Complex Parameters in V3


```yaml
openapi: 3.0.0
info:
  title: A map of parameter values
  version: 1.0.0
paths:
  /customers:      # https://api.example.org/customers?active=true&country=Canada&category=first
    parameters:
    - name: complexQuery
      in: query
      content:
        application/sparql-query:
          schema:
            type: string
          example: |-
            SELECT ?title
            WHERE
            {
              <http://example.org/book/book1> <http://purl.org/dc/elements/1.1/title> ?title .
            }
```
