# Embedded Server

One of the most useful features of CommandBox is the ability to start an ad-hoc server quickly and easily.  Any folder on your hard drive can become the web root of a server.  To start up the server, `cd` into a directory containing some CFML code, and run the `start` command.  An available port will be chosen by default and in a few seconds, a browser window will open showing the default document (`index.cfm`).

```bash
CommandBox> cd C:\sites\test
CommandBox> start
```

To stop the embedded server, run the `stop` command from the same directory.

```bash
CommandBox> stop
```

## OS Integration

You can start as many embedded server instances as you want.  Each running server will add a little green "Ortus" icon in your system tray.  Right click on it for options:

* Stop Server
* Open Browser
* Open Admin

 <img src="../images/embedded_server/system_tray_server_icons.png" alt="System Tray Server Icons">

## Full  Control

CommandBox's embedded server does not require any prior installations of any CFML engine to work.  It does not use Apache, IIS, or Nginx.  A very lightweight Java web server called [Undertow](http://undertow.io/) is used and a context is programmatically deployed via a WAR file.

You should still have all the options you need to set up most local development servers quickly.  The web-based administrator is available to you where you can edit any setting, add data sources, CF mappings, and mail servers.  To see a list of all the parameters you can pass to the `server start` command, refer to the [CommandBox API Docs](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/server/start.html) or run `server start help` command directly from the CLI.

