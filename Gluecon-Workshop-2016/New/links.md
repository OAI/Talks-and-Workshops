# Links

Links are a new mechanism in V3 that allows an API designer to describe relationships between operations.  It is possible to describe how values that are returned from one operation can be used to satisfy the parameters of a related operation.

!!!THIS IS NOT HYPERMEDIA!!!

## Motivation
There are two primary benefits to this feature.  Client generators are able to create client object models that can simplify the process of traversing a graph of objects.  Sometimes it is not desirable to retrieve an entire subtree in a single operation and higher level node needs to be accessed to determine if further requests are necessary. 

The other API artifact that can be improved with links is documentation.  Current API documentation based on OpenAPI v2 is limited to being a flat list of operations that can be grouped by path or by tag.  By using links to connect related operations, users should be able to navigate to the documentation of those operations.  This should make it easier to produce documentation that is more use-case friendly rather than being simply reference documentation.

## The simplest link

```yaml
openapi: 3.0.0
info:
  title: Simplest Link
  version: 1.0.1
paths:
  '/users/{username}':
    get: 
      parameters:
        - name: username
          in: path
          required: true
          schema:
            type: string
      responses:
        '200': 
          description: A representation of a user
          content: 
            application/json: 
              schema:
                type: object
                properties:
                  id: 
                    type: integer
                  firstname:
                    type: string
                  lastname:
                    type: string
              example:
                id: 243
                firstname: Reginald
                lastname: McDougall
          links:
            userPhotoLink:
              operationId: getPhoto
              parameters:
                userid: $response.body#/id
  '/users/{userid}/photo':
    get: 
      operationId: getPhoto
      parameters:
        - name: userid
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: A photo image
          content:
            image/jpeg: {}

```

## Self Referencing Links

There is nothing stopping links being used to reference the same operation and use the link identity to convey additional information about the relationship between the context and target operation.

```yaml
openapi: 3.0.0
info:
  title: Employees and Managers
  version: 1.0.1
paths:
  '/employees/{id}':
    get:
      operationId: getEmployeeById
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '200': 
          description: A representation of an employee
          content: 
            application/json: 
              schema:
                type: object
                properties:
                  id: 
                    type: string
                  username:
                    type: string
                  managerId:
                    type: string
          links:
            userManager:
              operationId: getEmployeeById
              parameters:
                id: $response.body#managerId
```
