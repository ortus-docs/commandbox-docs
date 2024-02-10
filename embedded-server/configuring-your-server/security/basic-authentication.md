# Basic Authentication

CommandBox's web server supports enabling Basic Auth on your sites.

```
server set web.security.basicAuth.enabled=true
server set web.security.realm="My Realm"
server set web.security.basicAuth.users.brad=pass
server set web.security.basicAuth.users.luis=pass2
```

That will create the following data in your `server.json`, which will be picked up the next time you start your server.

```javascript
{
    "web":{
        "security" : {
            "realm" : "My Realm",
            "authPredicate" : "regex( pattern='^/lucee/admin/.*', case-sensitive=false )",
            "basicAuth":{
                "users":{
                    "brad":"pass",
                    "luis":"pass2"
                },
                "enable":"true"
            }
        }
    }
}
```

If there is no `authPredicate` set, basic auth with secure ALL PAGES on the site. Once you set an `authPredicate`, only the pages matching the predicate will require authentication.

The old setting location for Basic Auth (`web.basicAuth`) will STILL WORK until the next major version of CommandBox, but should be considered deprecated. If both the settings exist (Ex: `web.basicAuth.enable` and `web.security.basicAuth.enable`), the new location will be given precedence.

{% hint style="info" %}
For [Multi-Site](../../multi-site-support/), any basic auth settings can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.
{% endhint %}

