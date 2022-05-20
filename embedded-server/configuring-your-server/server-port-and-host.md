# Server Port and Host

The `start` command will scan your system and find a random port that is not currently in use to start the server on. This ensures that multiple embedded servers can run at the same time on the same host without collisions. Ensure any redirects in your applications take the port into account.

## Any Port in the Storm

You may want to set a specific port to use-- even port 80 if nothing else is using it. Pass the HTTP port parameter to the start command like so:

```bash
 start port=8080
```

It is also possible to save the default port in your `server.json`. Add a `web.http.port` property, or issue the following command:

```bash
server set web.http.port=8080
server show web.http.port
```

Now every time you `start` your server, the same port will be used.

If the server won't start or is unreachable, make sure it's port is free with your operating system's `netstat` command. On Unix-based OS's:

```bash
 $> netstat -pan | grep 80
```

### HTTPS

You can start your server to listen for SSL connections too.

```bash
start SSLEnable=true SSLPort=443
```

```bash
server set web.SSL.enable=true
server set web.SSL.port=8080
server show web.SSL.enable
server show web.SSL.port
```

#### Setting SSL Enabled protocols

You can customize what SSL protocols your HTTPS listener will respond to with the following XNIO option.  Supply a comma-delimited list of valid protocols. &#x20;

```
server set runwar.XNIOOptions.SSL_ENABLED_PROTOCOLS=TLSv1.3,TLSv1.2
```

### HTTP/2

HTTP/2 is a newer standard of HTTP supported by all modern browsers.  HTTP/2 is enabled by default any time you are using an HTTP/HTTPS listener, however all major browsers will only allow the server to negotiate HTTP/2 over an HTTPS connection.  HTTP/2 runs over the same port and only changes the exchange between the server and browser.  You can disable HTTP/2 support like so:

```bash
server set web.http2.enable=false
```

If you want to confirm whether your browser is using HTTP/2, you can open your debugging tools and look at the network requests.  You may need to add the "protocol" column in the UI.  HTTP/2 will usually show up as something like "h2" in the protocol column.

### AJP

You can start your server to listen for AJP connections too.

```bash
start AJPEnable=true AJPPort=8009
```

```bash
server set web.AJP.enable=true
server set web.AJP.port=8009
server show web.AJP.enable
server show web.AJP.port
```

#### AJP Secret

CommandBox's AJP listener (provided by Undertow) is already protected against the [Ghostcat vulnerability](https://www.synopsys.com/blogs/software-security/ghostcat-vulnerability-cve-2020-1938/).  However, if you would like to set up an AJP secret as well to ensure all requests coming into the AJP listener are from a trusted source, you can do by setting the `web.ajp.secret` property.

```bash
server set web.AJP.secret=mySecret
```

For this to work, you must also configure your AJP proxy in your web server to send the same secret!  For requests received to the AJP listener which do not contain the secret, a `403` status code will be returned.  You can customize the output of the 403 page via the [Error Pages](custom-error-pages.md) settings.&#x20;

The AJP secret is implemented via a [Server Rule](server-rules/).  Feel free to add your own server rule instead of this setting if you want to customize how it works.

```javascript
equals(%p, 8009) and not equals(%{r,secret}, 'mySecret') -> set-error(403)
```

## A Gracious Host

Your application may rely on a specific host name other than the default of `127.0.0.1`. You can set the host to anything you like, but you must add a `host` file entry that resolves your host name to an IP address assigned to your network adapter (usually 127.0.0.1)

```bash
 start host=mycoolsite.local
```

If you have multiple IP addresses assigned to your PC, you can bind the server to a specific IP using the `host` parameter.

```bash
 start host=192.168.10.15 port=80
```

A server configuration can only have one host entry. If you require your server to be available on multiple IP addresses of the machine it runs on, you can set the host to 0.0.0.0. This will effectively bind the server to all network interfaces (including local).

```bash
 start host=0.0.0.0 port=80
```

Or save in `server.json`

```bash
server set web.host=mycoolsite.local
server show web.host
```

Most modern browsers allow you to make up any subdomain you want before localhost such as `mySite.localhost` and will simply resolve them to `localhost` (`127.0.0.1`) even without a hosts file entry.  CommandBox now supports using these domains and will bind your server's ports to localhost even without using the `commandbox-hostupdater` module.

```
server set web.host=mySite.localhost
```

## Customize URL that opens for server

By default, CommandBox will open your browser with the host and port of the server. You can customize the exact URL that opens. This setting will be appended to the current host and port.

```
server set openBrowserURL=/bar.cfm
```

Or you can completely override the URL if your setting starts with `http://`.

```
server set openBrowserURL=http://127.0.0.1:59715/test.cfm
```
