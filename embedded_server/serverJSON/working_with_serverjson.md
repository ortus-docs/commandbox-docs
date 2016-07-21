
## Working With Server.json

Interacting with the `server.json` file uses the commands `server set`, `server show`, and `server clear`, which work the same as the `package set/show/clear` commands.

Set the port for your server:
```bash
server set web.http.port=8080 
```

View the port:

```bash
server show web.http.port
```

Remove the saved setting:

```bash
server clear web.http.port
```

For completeness, here is the full list of all `server.json` properties that can be set.  This is a flattened version of the JSON at the top of this page:

```
stopsocket
trayicon
name
debug
openbrowser
jvm.heapsize
jvm.args
web.host
web.webroot
web.directorybrowsing
web.http.port
web.http.enable
web.ssl.cert
web.ssl.keypass
web.ssl.key
web.ssl.port
web.ssl.enable	
web.rewrites.config
web.rewrites.enable
app.webxml
app.logdir
app.serverconfigdir
app.libdirs
app.webconfigdir
app.WARPath
app.cfengine
runwar.args
```