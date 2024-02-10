# Multi-Site Support

The default behavior when you run `server start` is to start a server that points only to a single web root.  There are two methods you can use to host more than one web root at a time in a single CommandBox server:

* [**ModCFML**](../modcfml-support.md)**-** this requires a web server sitting in front of CommandBox which sends special HTTP headers that instruct CommandBox what to use as the web root.
* **Multi-Site mode**- this is new in CommandBox 6.0 and it allows full configuration of multiple sites (web roots), rewrite, HTTP/SSL bindings, and settings inside of a single server.&#x20;

In both cases, there is a single JVM process and a single CF Engine at play.  So all sites will use the same CF engine and version.  If you want to run different versions of CF, you'll need more than one server.  You can use a reverse proxy in your server rules to still access all your sites via a single "front end" server instance. &#x20;

The CommandBox built-in web server is now fully fledged with out-of-the-box support for multiple bindings, host header matching, multiple SSL certs/SNI, and full control over rewrites gzip compression, virtual directories (aliases) and secure profiles on a per-site basis.  CommandBox is now truly the only tool you need to deploy your code!  There's no need to have Apache, IIS, or Nginx in the mix any longer. &#x20;

There is a repo on Github that contains working examples of all the features of multi-site.  Feel free to skip straight there to learn by example.

[https://github.com/Ortus-Solutions/commandbox-tests](https://github.com/Ortus-Solutions/commandbox-tests)

{% hint style="info" %}
You are free to use CommandBox as you with for development purposes, but if you are using more than 2 sites in production, we ask you consider supporting us with a [CommandBox Pro](https://www.ortussolutions.com/products/commandbox-pro) license which bundles support and commercial modules and extensions.
{% endhint %}

### Tuckey Rewrites

Please note that the [legacy form of rewrites](./#tuckey-rewrites) using Tuckey will NOT work by default in Multi-Site mode.  There are two main issues

* Tuckey is tied to the servlet and static files are now no longer served by the Default Servlet, but are returned to the browser without even touching the servlet.
* There is only one servlet servicing all sites, so you can't have different rewrites per site

As a workaround if you're not ready to leave Tuckey, you can force the new Undertow web server to not serve static files by forcing the `servletPassPredicate` to send ALL files to the servlet:

```bash
server set web.servletPassPredicate=true
```

Read more about the [servlet pass predicate here](servlet-pass-predicate.md).

However, the best approach is to simply switch over to using [Server Rules](../configuring-your-server/server-rules/).  They do everything Tuckey does, AND each site can get its own separate set of server rules for maximum configurability.

Also note, when you set this flag:

```bash
server set web.rewrites.enable=true
```

as of CommandBox 6, a Tuckey XML rewrite file will no longer be employed.  Instead Server Rules will be added that perform the same rewrites as the old Tuckey functionality.  Tuckey will now ONLY be used if you specify a custom rewrite file.

