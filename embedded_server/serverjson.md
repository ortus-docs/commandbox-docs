# server.json

Every time you start a server, the settings used to start it are saved in a `server.json` file in the web root.  Any parameters that aren't supplied to the `start` command are read from this file (if it exists) and used as defaults.  A sample `server.json` file could look like this:

**`server.json`**
```javascript
{
    "heapSize":"1024",
    "host":"localhost",
    "rewritesEnable":true,
    "port":"8081"
}
```

## Server Setting Order

Settings are loaded in this order:

1. Typed by the user into the `start` command parameters
2. Read from `server.json`
3. Internal defaults from the `ServerService`

## Packaging Your Server

This allows you to package up an app that requires special start settings such as rewrites, JVM args, or heap size, and anyone can run it with the same settings you do by simply typing `server start`.   Make sure to not deploy the `server.json` file to your production server where it may be web-accessable.

## Working With Server.json

Interacting with the `server.json` file uses the commands `server set`, `server show`, and `server clear`, which work the same as the `package set/show/clear` commands.

Set the port for your server:
```bash
server set port=8080
```

View the port:

```bash
server show port
```

Remove the saved setting:

```bash
server clear port
```