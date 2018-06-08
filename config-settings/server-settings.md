# Server Settings

These settings control how servers start in CommandBox.

## server.defaults

**struct**

This struct can contain [any setting that is valid in a `server.json` file](https://github.com/ortus-docs/commandbox-docs/blob/master/embedded-server/server.json/README.md). These settings are used as global default settings if there is not a corresponding setting provided by the user via a parameter to the `start` command or in the server's `server.json` file.

```bash
config set server.defaults.web.rewrites.enable=true
config set server.defaults.openbrowser=false
config set server.defaults.jvm.heapsize=1024
config show server.defaults
```

