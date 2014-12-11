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

## Full  Control

You have full control of your server. The web-based administrator is available to you where you can edit any setting, add data sources, CF mappings, and mail servers.

CommandBox's embedded server does not require any prior installations of any CFML engine to work.  It does not use Apache, IIS, or NGinX.  A very lightweight Java web server called [Undertow](http://undertow.io/) is used and a context is programmatically deployed via a WAR file.



## OS Integration

You can start as many embedded server isntances as you want.  Each running server will add a little green "Ortus" icon in your system tray.  Right click on it for options:

* Stop Server
* Open Browser
* Open Admin
