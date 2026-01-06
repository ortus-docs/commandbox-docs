---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/Bw6M3PI3e5HgLVcKZz0G/embedded-server/configuring-your-server/url-rewrites
---

# URL Rewrites

Once you start using the embedded server for your development projects, you may wish to enable URL rewriting. Rewrites are used by most popular frameworks to do things like add the `index.cfm` back into SES URLs.

Commandbox exposes a way to do url rewrites with the Undertow predicate language. If you missed the [Server Rules](../server-rules/) section, go there to learn how to do url rewrites, security, and http header modification in a nice text based language (non-xml).

{% hint style="warning" %}
Note: [Tuckey-based URL rewrites](tuckey-rewrites.md) are not recommended going forward.  They are limited and do not play with with [Multi-Site](../../multi-site-support/) servers.  It is recommended you move to the more-powerful [Server Rules](../server-rules/).
{% endhint %}

## Default Rules

We've already added the required jars and created a default rewrite [XML file](http://cdn.rawgit.com/paultuckey/urlrewritefilter/master/src/doc/manual/4.0/index.html#filterparams) that will work out-of-the-box with the ColdBox MVC Platform. To enable rewrites, start your server with the `--rewritesEnable` flag.

```bash
start --rewritesEnable
```

Now URLs like

```
http://localhost/index.cfm/main
```

can now simply be

```
http://localhost/main
```

In `server.json`

```bash
server set web.rewrites.enable=true
server show web.rewrites.enable
```



## SES URLs

Your servers come ready to accept SES-style URLs where any text after the file name will show up in the `cgi.path_info`. If rewrites are enabled, the `index.cfm` can be omitted.

```
site.com/index.cfm/home/login
```

SES URLs will also work in a sub directory, which used to only work on a "standard" Adobe CF Tomcat install. Please note, in order to hide the `index.cfm` in a subfolder, you'll need a custom rewrite rule.

```
site.com/myFolder/index.cfm/home/login
```

## Logging

Undertow's Predicate Language has debug and trace level logging that can help you troubleshoot why your rewrite rules aren't (or _are_) firing. To view these logs, simply start your server with the `--debug` or `--trace` flags. Trace shows more details than debug. These options work best when starting in `--console` mode so you can watch the server logs as you hit the site. Alternatively, you can follow the server's logs with the `server log --follow` command.

```
start --debug
server log --follow
```

Note, a lot of baked-in CommandBox features use server rules behind the scenes, so your debug/trace logging will reflect ALL rules which are running on every request, not just your custom server rules.
