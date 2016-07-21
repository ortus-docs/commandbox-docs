# Packaging Your Server

`server.json` allows you to package up an app that requires special start settings such as rewrites, JVM args, or heap size, and anyone can run it with the same settings you do by simply typing `server start`.   Make sure to not deploy the `server.json` file to your production server where it may be web-accessible.

### Storing `server.json` outside the web root

To help with this, you can store your `server.json` file outside of the web root and use the `web.webroot` property in it to point to the location of the web root. This can be an absolute path or a  relative path to the location of the JSON file.

```bash
server set web.webroot=www
```

