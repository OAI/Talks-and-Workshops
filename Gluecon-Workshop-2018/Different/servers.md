# Servers

## OpenAPI V2

In V2 the server where the API was hosted was defined by three root level properties that were combined to form the API root URL.

```yaml
swagger: '2.0'
info:
  title: How server used to be identified
  version: 1.0.0

basePath: /api
host: www.acme.com
schemes: ["https"]

paths: {}

```

## OpenAPI V3
In V3 there is a `servers` array that allows defining multiple base URLs

```yaml
openapi: 3.0.0
info:
  title: How servers are identified in V3
  version: 1.0.0

servers:
- url: https://prod.acme.com/api
- url: https://sandbox.acme.com/api

paths: {}
```

## Templated Servers

```yaml
openapi: 3.0.0
info:
  title: How server URLs can be templated
  version: 1.0.0

servers:
- url: https://{tenant}.acme.com/api/{version}
  variables:
    tenant:
      default: sample  #default is a required field
    version:
      default: v2
paths: {}
```

When the server URL is a relative reference it should be resolved using the location of the OpenAPI definition as base URL.  When the servers property is absent, the base path of the API is assumed to be at the root of the host where the OpenAPI document is located.
