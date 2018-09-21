# Callbacks

## The simple Webhook

In this example API, the operation allows a client to subscribe to a webhook by POSTing a JSON representation that has the callback URL as a property of the response body.  The operation has a `callback` object called `mainHook` that describes the HTTP request that will be made when the webhook is triggered by the origin server.

```yaml
openapi : 3.0.0
info:
  title: A simple webhook subscription
  version: 1.0.0
paths: 
  '/subscribe':
    post:
      requestBody:
        content:
          application/json: 
            schema:
              type: object
              properties:
                url: 
                  type: string
                  format: uri 
      responses:
        '201':
          description: Created subscription to webhook
      callbacks:
        mainHook:
          '$request.body#/url':
            post:
              responses:
                '200':
                  description: webhook successfully processed
```

## Enhanced Webhook

Enhancing the webhook to allow unsubscribing using a link.

```yaml
openapi: 3.0.0
info:
  title: Example of links with callbacks
  version: 0.9.0
paths:
  /hooks:
    post:
      requestBody:    
        description: body provided by consumer with callback URL
        required: true
        content:
          application/json:
            example:
              callback-url: https://consumer.com/hookcallback
      responses:      
        '201':
          description: Successfully subscribed
          content:
            application/json:
              example:
                hookId: 23432-32432-45454-97980
          links:
            unsubscribe:
              operationId: cancelHookCallback
              parameters: 
                id: $response.body#/hookId
      callbacks:
        hookEvent:
          '$request.body#/callback-url':
            post:
              requestBody:
                content:
                  application/json:
                    example:
                      message: Here is the event
              responses:
                '200':
                  description: Expected responses from callback consumer
  /hooks/{id}:
    delete:
      operationId: cancelHookCallback
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Successfully cancelled callback

```

## Async Callback
An operation can support multiple different callback operations.  This enables a server to signal different operation outcomes by calling back to different resources with different messages.  The initiating call can provide the callback URLs in a variety of different locations.  A new runtime expression syntax can be used to obtain these URLs from URL parameters, headers, request and response bodies.

```yaml
openapi: 3.0.0
info:
  title: Example of links with callbacks
  version: 0.9.0
paths:
  /longrunning-thing:
    post: 
      parameters:
        name: heartbeat-url
        in: query
        schema:
          type: string
          format: uri
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                failedUrl:
                  type: string
                successUrl:
                  type: string
      responses:
        '200': 
          description: Long running thing initiated
      callbacks:
        heartbeat:
          '$request.query.heartbeat-url':
            post:
              requestBody:
                $ref: '#/components/requestBodies/heartbeatMessage'
              responses:
                '200':
                  description: Consumer acknowledged the callback   
        failed:
          '$response.body#/failedUrl':
            post:
              requestBody:
                $ref: '#/components/requestBodies/failedMessage'
              responses:
                '200':
                  description: Consumer acknowledged the callback   
        success:
          '$response.body#/successUrl':
            post:
              requestBody:
                $ref: '#/components/requestBodies/successMessage'
              responses:
                '200':
                  description: Consumer acknowledged the callback   
```
