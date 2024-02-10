# Performance Tuning

Here are some settings for performance tuning your servers

## Max Requests

This setting limits how many requests will be passed through the Undertow listener to your app server.  It defaults to 30. Please note that your web server connector and Adobe ColdFusion may also have their own max request setting. &#x20;

```bash
server set web.maxRequests=100
```

Or as a global setting for all servers:

```bash
config set server.defaults.web.maxRequests=100
```

{% hint style="danger" %}
Max Requests can NOT be set on a per-site basis since this setting controls the size of Undertow's worker task pool.  You can, however, use the `request-limit( 10 )` handler in a server rule that applies only to a specific site.  Keep in mind, the maxRequests setting will still enforce an overall total running threads for all sites on a server.
{% endhint %}
