# GZip Compression

The web server in CommandBox is capable of enabling GZIp compression to reduce the size of HTTP responses.  To enable GZip compress on your CommandBox server, add a `web.gzipEnable` setting in your `server.json` file.

```bash
server set web.gzipEnable=true
```

By default GZip compression will be applied to any file over 1500 KB in size.  This is because 1500 KB or smaller can fit inside a single network packet, so there's no real benifit in zipping small files and it just causes CPU overhead.  

If you wish to customize the file size or any other aspect of when GZip compression is applied, you can specify a custom [Undertow Predicate](server-rules/rule-language.md) that, when true, will trigger GZip for the request.

```bash
server set web.gzipPredicate="request-larger-than(500)"
```

Since ANY valid Undertow predicate language can be used here you can combined predicates to control exactly when GZip compression is applied.  This example would apply GZip compression to any CSS files over 500 KB that were not in the /admin folder.

```bash
server set web.gzipPredicate="not path-prefix( admin ) and regex( '(.*).css' ) and request-larger-than(500)"
```

