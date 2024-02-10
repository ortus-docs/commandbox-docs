# Open Browser URL

## Customize URL that opens for server

By default, CommandBox will open your browser with the host and port of the server. You can customize the exact URL that opens. This setting will be appended to the current host and port.

```
server set openBrowserURL=/bar.cfm
```

Or you can completely override the URL if your setting starts with `http://`.

```
server set openBrowserURL=http://127.0.0.1:59715/test.cfm
```

{% hint style="info" %}
For [Multi-Site](../../multi-site-support/), the openBrowserURL can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.  The browser will not open automatically, but this setting will control the options in the tray menu and the `server open` command.
{% endhint %}
