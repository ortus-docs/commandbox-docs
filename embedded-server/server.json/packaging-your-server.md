# Packaging Your Server

`server.json` allows you to package up an app that requires special start settings such as rewrites, JVM args, or heap size, and anyone can run it with the same settings you do by simply typing `server start`. Make sure to not deploy the `server.json` file to your production server where it may be web-accessible.

### Storing `server.json` outside the web root

To help with this, you can store your `server.json` file outside of the web root and use the `web.webroot` property in it to point to the location of the web root. This can be an absolute path or a relative path to the location of the JSON file.

```bash
server set web.webroot=www
```

When you start the server, you can run the `start` command from the same directory that the `server.json` file lives, or specifiy the path to the JSON file like so:

```bash
start /path/to/server.json
```

## Determining the web root

If there is no web root in your `server.json`, CommandBox will use the folder that the JSON file is stored in. If there is no JSON file at all, the current working directory is used.
