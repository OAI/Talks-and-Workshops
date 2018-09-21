# Examples
There have been a variety of changes to the way Examples are defined in OpenAPIV3.  Examples for request and response bodies are now defined with a media type object inside a content object and therefore there is no longer need for the Examples map that used the media type identifier as a key. 
Within a content object there are two options for defining an example. The quick way and the long way.  The quick way is a simple `example` property where the value is a literal example.  The long way uses an `Example Object` which has a richer set of properties.  With an `Example Object` the example has an identifier uses as the map key.  There are summary and description properties and the example value can point too an example defined elsewhere.

# Multiple Media Types and Multiple Examples

```yaml
openapi: 3.0.0
info:
  title: Examples of the new example object
  version: 1.0.0
paths:
  '/names':
    get:
      responses:
        '200':
          description: OK
          content:
            'application/json': 
              schema:
                type: array
                items:
                  type: string
              examples:
                list:
                  summary: List of Names
                  description: |- 
                                    A long and _very_ detailed description of this representation that includes rich text.
                  value:
                    - Bob
                    - Diane
                    - Mary
                    - Bill
                empty:
                  summary: Empty
                  value: [ ]
            'application/xml': 
              examples:
                list:
                  summary: List of names
                  value: "<Users><User name='Bob'/><User name='Diane'/><User name='Mary'/><User name='Bill'/></Users>"
                empty:
                  summary: Empty list
                  value: "<Users/>"
            'text/plain':
              examples:
                list:
                  summary: List of names
                  value: "Bob,Diane,Mary,Bill"
                empty:
                  summary: Empty
                  value: ""
```

# Referencing Examples

```yaml
openapi: 3.0.0
info:
  title: Examples of the new example object using a $ref and a external link
  version: 1.0.0
paths:
  '/pets':
    get:
      responses:
        '200':
          description: OK
          content:
            application/json: 
              schema:
                $ref: '#/components/schemas/Pet'
              examples:
                dog:
                  summary: An example of a dog with a cat's name
                  value:
                    name: Puma
                    petType: Dog
                    color: Black
                    gender: Female
                    breed: Mixed
                frog:
                  $ref: '#/components/examples/frog-example'
                cat:
                  summary: An example of a cat
                  externalValue: '/examples/cat.json'
```
