# Resource Manager Logging

CommandBox uses a custom resource manager in Undertow to resolve "real" paths (relative) in the servlet context with the path on the file system (absolute).  This includes log concerning where the root of the WAR is, where the CF engine web root is, and any web aliases.  If you enable a `--debug` or `--trace` server start, it is possible to get additional low-level information in the logs about every single file path that is resolved through the servlet context's resource manager.  This logging is disabled by default since it is quite verbose.

```bash
server set web.resourceManagerLogging=true"
```

Or in the `server.json` as:

{% code title="server.json" %}
```javascript
{
  "web" : {
    "resourceManagerLogging" : "true"
  }
}
```
{% endcode %}

{% hint style="info" %}
For [Multi-Site](../multi-site-support/), Resource Manger Logging can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.
{% endhint %}
