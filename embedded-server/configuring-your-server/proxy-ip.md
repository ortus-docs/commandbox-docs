# Proxy IP

If CommandBox is running a server behind a trusted proxy which is known to set the `X-Forwarded-For` HTTP  header, then you can enable this setting for the remote IP in your CF engine's `cgi` scope to represent the upstream IP.

```
server set web.useProxyForwardedIP=true
```

&#x20;This is disabled by default.  Otherwise a malicious client could send a fake `X-Forwarded-For` HTTP  header to make it look like their traffic was coming from a trusted IP such as `127.0.0.1` to bypass any IP-based restrictions.   Only enable this if you trust the proxy CommandBox is sitting behind.  This should never be enabled if CommandBox is directly accessible on the internet. &#x20;

Enable for all servers like so:

```
config set server.defaults.web.useProxyForwardedIP=true
```
