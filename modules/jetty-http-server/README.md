# HTTP Server

The `http-server` module lets you run a HTTP server.

## Configuration

key | type | required | note
--- | --- | --- | --- 
`http.server.port` | int | optional | ie 8080
`http.server.address` | string | optional | default 0.0.0.0
`http.server.ttlMillis` | int | optional | default 30000, determines how long before an error response is sent

## Special request headers

Header | Effect
--- | ---
`X-Calling-Service` | Accessible through `Request#service`

## Example
This example shows how to start an http server using the HttpServerModule from apollo-core.

```java
  public static void main(String... args) throws IOException, InterruptedException {
    Service service = Services.usingName("test")
        .withModule(HttpServerModule.create())
        .build();

    try (Service.Instance instance = service.start(args)) {
      RequestHandler handler = new Handler();
      HttpServer httpServer = HttpServerModule.server(instance);

      httpServer.start(handler);
      instance.waitForShutdown();
    }
  }

  public static class Handler implements RequestHandler {

    public void handle(OngoingRequest request) {
      request.reply(Response.ok());
    }
  }
```

The http server will start if the `http.port` config key is set.

