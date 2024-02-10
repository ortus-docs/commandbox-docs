# Welcome Files

The default welcome files are the usual index.cfm, index.htm, index.html, etc but you can override this with the `welcomeFiles` setting in your `server.json` by providing a comma-delimited list of files that you would like CommandBox to look for when a user hits a directory on a running server.

```
server set web.welcomeFiles='go.cfm,main.cfm,index.html'
```

This setting is a complete override of the defaults, so you need to specify the full list.

## Directory Browsing

By default, a CommandBox server will not show the contents of a directory that doesn't have an index file.  You can enable directory browsing for a single server with

```bash
server set web.directoryBrowsing=true
```

And you can enable it for all servers by default with

```bash
config set server.defaults.web.directoryBrowsing=true
```

{% hint style="info" %}
For [Multi-Site](../multi-site-support/), Welcome File settings can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.
{% endhint %}

