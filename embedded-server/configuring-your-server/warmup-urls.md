---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/Bw6M3PI3e5HgLVcKZz0G/embedded-server/configuring-your-server/warmup-urls
---

# Warmup URLs

If you want to warm up the server as soon as it comes online, CommandBox has the ability to specify one or more warm up URLs for to fire after the server is started.  This can be used to warm up caches, or even fire onServerStart listeners.&#x20;

The feature is implemented as a [Server Rule](server-rules/) that accepts

* `urls` – array of URLs to hit (_required_)
* `requestStrategy` – during warmup…
  * `block` URLS with 503
  * `queue` requests until the server is ready (at which point they will be processed)
  * `allow` requests to flood in before warmup is complete (_default_)
* `async` -- fire warmup URLs one at a time, or all at once (_default to_ `true`)
* `timeoutSeconds` -- how long to wait for the warm up URLs (_default to_ `60`)

### Async

If `async` is false, the timeout is applied separately to each URL. So, each URL is given the full timeout. If `async` is true, the timeout simply begins when they are all fired and applies to all of them at the same time. When the timeout is reached, we don’t end the request. It will still load in the background, but if the `requestStrategy` is block or queue, we will no longer block or queue new traffic.

### Request Queue

The queue size is 10,000 and cannot be changed right now. That means up to 10,000 requests can come into the server while it is warming up and be queued. Once the queue is full, a 503 status code will be immediately returned for any additional traffic, which is the same as the block strategy. The first 10,000 requests in the queue will still process once the server is warmed up.

The request strategies of block and allow do not use the queue at all.&#x20;

### Configuration

You can set up a simple single-URL setup, which doesn't block or queue by default like so. The example uses positional args and only has a single URL so the `{}` array notation is not needed.

**server.json**

```json
{
    "web":{
        "rules":[
            "warm-up-server( 'http://127.0.0.1:8080' )"
        ]
    }
}
```

A more complicated setup with more than one URL and custom settings. This uses named params and has more than one URL, so we use the `{}` notation.

```json
{
    "web":{
        "rules":[
            "warm-up-server( urls={'https://cnn.com','http://www.google.com','http://127.0.0.1:8080/warmup.cfm?brad=wood'}, requestStrategy=queue, async=false, timeoutSeconds=10 )"
        ]
    }
}
```

URls can also be a relative URI, and the default base URL for that site (the same as what we would use to open the browser) will be used to create the complete URL. This can be handy for a site with uses different ports or hostnames across environments.

```json
{
    "web":{
        "rules":[
            "warm-up-server( '/myfile.cfm?foo=bar' )"
        ]
    }
}
```
