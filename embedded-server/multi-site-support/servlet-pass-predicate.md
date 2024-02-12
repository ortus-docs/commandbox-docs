---
description: Use a servlet pass predicate to define which requests should be forwarded to the servlet
---

# Servlet Pass Predicate

In a traditional server setup, you are used to two separate pieces of software

* A web server like NGinx, Apache, or IIS which serves static files, performs URL rewrites, and proxies some requests off to a servlet
* A Servlet container like Tomcat or CommandBox receiving the proxied requests on another port (HTTP or AJP) and processing CF code

In CommandBox, now we have the ability to combine both of those bullets together into a single piece of software.  No reverse proxy is needed, nor is a second port to proxy to.

In all cases with the legacy setup, there is always a check of some kind that decides which requests will get sent to the servlet.  This is usually checking for URLs ending in `.cfm` or `.cfc`.  Common implementations of this are:

* Adobe's CF connector
* The mod\_cfml module for Apache
* BonCode for IIS
* A custom proxy directive in your web server.

As of CommandBox 6, we have the same setting and it can be set as `web.servletPassPredicate` which is simply an [Undertow Predicate](../configuring-your-server/server-rules/).  By default, CommandBox uses the following predicate for the `servletPassPredicate`:

```javascript
regex( '^/(.+?\\.cf[cm])(/.*)?$' )
```

So the URI `/index.cfm` will get sent to the servlet, but `/test.txt` would just get sent by the static file resource handler.   You can customize this if necessary with any valid Undertow predicate.  It can even be different on a per-site basis by setting it inside the `sites` object or `.site.json` file for a given site.

```bash
# Send ALL traffic to the servlet (the servlet's default handler will serve static files)
server set web.servletPassPredicate=true

# Only serve CF files out of the cgi folder
server set web.servletPassPredicate=regex( '^/cgi/(.+?\\.cf[cm])(/.*)?$' )

# process CFM files or anything in the /REST/ directory
server set web.servletPassPredicate=regex( '^/(.+?\\.cf[cm])(/.*)?$' ) or path-prefix-nocase( /REST/ )
```

Note, be careful as your servlet pass setting may send `.cfm` files to the static file handler.  CommandBox has a [failsafe in place](../configuring-your-server/server-rules/allowed-static-files.md) to prevent the source code from being returned, but you should still use caution when configuring this.
