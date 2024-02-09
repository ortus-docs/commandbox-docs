# Proxy Settings

If you need to use CommandBox behind a corporate proxy, these settings will be necessary for it to successfully connect to the Internet.

## proxy.server

**string**

This is the URL of the proxy server on your network.

```bash
config set proxy.server=myProxy.com
config show proxy.server
```

## proxy.port

**integer**

This is the port to connect to on the proxy server.

```bash
config set proxy.port=9000
config show proxy.port
```

## proxy.user

**string**

This is the username to connect to the proxy server with, if required.

```bash
config set proxy.user=proxyUser
config show proxy.user
```

## proxy.password

**string**

This is the password to connect to the proxy server with, if required.

```bash
config set proxy.password=proxyPass
config show proxy.password
```
