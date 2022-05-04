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

