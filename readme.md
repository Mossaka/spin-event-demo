## CloudEvents Abuse Protection

The purpose of this example is to show that a simple spin component can be used to protect HTTP CloudEvents authentication.

> CloudEvents v1.0 implements its own [abuse protection semantics](https://github.com/cloudevents/spec/blob/v1.0/http-webhook.md#4-abuse-protection) using the HTTP OPTIONS method.

> An example of the abuse protection provider is the [Azure Event Grid](https://docs.microsoft.com/en-us/azure/event-grid/webhook-event-delivery) webhook service.

### build
- Run `spin build` to build the component in current directory.

### run & logging
- Run `RUST_LOG=spin=trace spin up -f spin.toml -L ./log` to run and log the component.

### curl
```
   curl -v "localhost:3000" \
    -X POST \
     -H "Ce-Specversion: 1.0" \
     -H "Ce-Type: dev.knative.samples.helloworld" \
     -H "Ce-Source: dev.knative.samples/helloworldsource" \
     -H "Ce-Id: 536808d3-88be-4077-9d7a-a3f162705f79" \
     -H "Content-Type: application/json" \
     -d '{"msg":"Hello spin!"}' 
```

### quick deploy using ngrok
- Run `ngrok http 3000` to start a local ngrok tunnel to the component.

### sample request

The following request is sent by [Azure Event Grid](https://docs.microsoft.com/en-us/azure/event-grid/webhook-event-delivery)
```rust
Request { 
    method: OPTIONS, 
    uri: /, version: HTTP/1.1, 
    headers: {
        "host": "<host-url>", 
        "webhook-request-callback": "<callback-url>", 
        "webhook-request-origin": "eventgrid.azure.net", 
        "x-forwarded-for": "20.150.164.0", 
        "x-forwarded-proto": "https", 
        "accept-encoding": "gzip", 
        "spin-path-info": "/", 
        "spin-full-url": "<host-url>", 
        "spin-matched-route": "", 
        "spin-base-path": "/", 
        "spin-raw-component-route": "/", "spin-component-route": "/"}, 
    body: Some(b"") }

```