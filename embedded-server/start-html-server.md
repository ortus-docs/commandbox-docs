# Start HTML Server

You may want to start up a local server that does not have a CF Engine such as Lucee or Adobe ColdFusion installed.  You can do this as of CommandBox 5.1.0 by setting the `cfengine` parameter to `none` like so:

```
server start cfengine=none
```

Or in your `server.json` like this

```
server set app.cfengine=none
server start
```

This server will support everything that you are used to including the `server.json` file, heap settings, ports, and virtual directories.  The only difference is it will server everything as static files.  (.cfm or .cfc files will not be processed)
