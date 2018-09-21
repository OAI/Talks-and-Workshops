# Schema Object

The Schema Object has expanded JSON Schema support, with additional extensions for the OpenAPI Specification.

-- Some of the examples are taken from https://spacetelescope.github.io/understanding-json-schema/reference/combining.html

## `anyOf`

We just need to match one of the sub-schemas:

```yaml
anyOf:
  - type: string
  - type: number
```


## `oneOf`

We need to match exactly one of the sub-schemas.

Not a good example:

```yaml
oneOf:
  - type: string
  - type: number
```

However:

```yaml
oneOf:
  - type: number
    multipleOf: 5
  - type: number
    multipleOf: 3
```


## `not`

If it's `not`, it's not...

```yaml
not:
  type: string
```

## `nullable`

Allowing to pass the value of `null` for a property.

```yaml
Record:
  type: object
  properties:
    id:
      type: integer
    color:
      type: string
      nullable: true
```

## Additional properties

- `writeOnly` - The opposite of `readOnly`, allows you to specify that a property can only be sent by the API consumer, but will not be sent by the producer (passwords...).
- `deprecated` - should be self explanatory 

