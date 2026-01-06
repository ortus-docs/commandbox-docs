---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/Bw6M3PI3e5HgLVcKZz0G/embedded-server
---

# Embedded Server

One of the most useful features of CommandBox is the ability to start an ad-hoc server quickly and easily. Any folder on your hard drive can become the web root of a server. To start up the server, `cd` into a directory containing some CFML code, and run the `start` command. An available port will be chosen by default and in a few seconds, a browser window will open showing the default document (`index.cfm`).

```bash
CommandBox> cd C:\sites\test
CommandBox> start
```

To stop the embedded server, run the `stop` command from the same directory.

```bash
CommandBox> stop
```

## OS Integration

You can start as many embedded server instances as you want. Each running server will add an icon in your system tray with the logo of your currently running engine. Click on it for options:

* Stop Server
* Open Browser
* Open Admin
* Open File System

![CommandBox Server Tray Menu](https://github.com/ortus-docs/commandbox-docs/tree/df981947c5780503203384f9de7118f57ee01ca5/.gitbook/assets/image%20\(5\).png)

### Open Browser

When a server finishes starting, CommandBox will automatically open up the root of the site in your default browser. You can control the URL that is opened with

```bash
server set openBrowserURL=/admin/foo.cfm
```

You can change the browser used to open all sites or override just the browser for a single site:

```bash
# Global setting
config set preferredBrowser=Chrome
# Per-server setting
server set preferredBrowser=Chrome
```

Or you can disable this feature entirely

```bash
# Global setting
config set server.defaults.openBrowser=false
# Per-server setting
server set openBrowser=false
```

### Disable the tray icon

If you don't want the tray integration, then you can turn it off in your `server.json` with this setting.

```
server set trayEnable=false
```

Or turn it off at a global level in your config settings.

```
config set server.defaults.trayEnable=false
```

## Full Control

CommandBox's embedded server does not require any prior installations of any CFML engine to work. It does not use Apache, IIS, or Nginx. A very lightweight Java web server called [Undertow](http://undertow.io/) is used and a context is programmatically deployed via a WAR file.

You should still have all the options you need to set up most local development servers quickly. The web-based administrator is available to you where you can edit any setting, add data sources, CF mappings, and mail servers. To see a list of all the parameters you can pass to the `server start` command, refer to the [CommandBox API Docs](http://apidocs.ortussolutions.com/commandbox/6.2.0/index.html?commandbox/system/modules_app/server-commands/commands/server/start.html) or run `server start help` command directly from the CLI.

## Environment Variables

Any ComandBox environment variables present in the shell will automatically be passed to the environment of the server process. This means, given an example like this:

```bash
set foo=bar
server start
```

The CFML code running that server process will be able to "see" the `foo` environment variable.

## Working Dir of Server process

The working directory of the Java server process will be the directory where the `server.json` file lives, if it exists.  Otherwise, it will be the webroot of the server (or the default site if multi-site).  This doesn't generally have much affect on CF servers, but it does affect how Java resolves relative paths and you can utilize it if you have relative paths in your config files, or want to use the `user.dir` Java system property as a config file placeholder.
