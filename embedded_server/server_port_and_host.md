# Server Port and Host

The `start` command will scan your system and find a random port that is not currently in use to start the server on.  This ensures that multiple embedded servers can run at the same time on the same host without collisions.  Ensure any redirects in your applications take the port into account.

## Any Port in the Storm

You may want to set a specific port to use-- even port 80 if nothing else is using it.  Pass the HTTP port parameter to the start command like so:

```bash
 start port=8080
```

It is also possible to save the default port in your `server.json`.  Add a `web.http.port` property, or issue the following command:

```bash
server set web.http.port=8080
server show web.http.port
```

Now every time you `start` your server, the same port will be used.

If the server won't start or is unreachable, make sure it's port is free with your operating system's `netstat` command.  On Unix-based OS's:

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

### AJP

You can start your server to listen for AJP connections too.

```bash
start AJPEnable=true AJPPort=8009
```

```bash
server set web.AJP.enable=true
server set web.AJP.port=8080
server show web.AJP.enable
server show web.AJP.port
```

## A Gracious Host

Your application may rely on a specific host name other than the default of `127.0.0.1`.  You can set the host to anything you like, but you must add a `host` file entry that resolves your host name to an IP address assigned to your network adapter \(usually 127.0.0.1\)

```bash
 start host=mycoolsite.local
```

If you have multiple IP addresses assigned to your PC, you can bind the server to a specific IP using the `host` parameter.

```bash
 start host=192.168.10.15 port=80
```

Or save in `server.json`

```bash
server set web.host=mycoolsite.local
server show web.host
```

## Customize URL that opens for server

By default, CommandBox will open your browser with the host and port of the server.  You can customize the exact URL that opens.  This setting will be appended to the current host and port.
```
server set openBrowserURL=/bar.cfm
```

Or you can completely override the URL if your setting starts with `http://`.

```
server set openBrowserURL=http://127.0.0.1:59715/test.cfm
```


