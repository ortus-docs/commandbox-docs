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

## Multi-Engine Support

You can specify the CFML engine via the command line arguments:

```bash
CommandBox> start cfengine=adobe@9
```

This will start an Adobe ColdFusion 9 server in your webroot.  That's it!

By default, CommandBox uses the `cfengine` slug to search for the engine on ForgeBox.  The format is `slug@version` where the version is optional.  Ortus Solutions maintains the versions of the engines available on ForgeBox.

Supported engines are:

* Adobe ColdFusion 9
* Adobe ColdFusion 10
* Adobe ColdFusion 11
* Adobe ColdFusion 2016
* Railo 4.2
* Lucee 4.5
* Lucee 5

Here are some examples:

```bash
# Start the default engine
CommandBox> start

# Start the latest stable Railo engine
CommandBox> start cfengine=railo

# Start a specific engine and version
CommandBox> start cfengine=adobe@10.0.12

# Start the most recent Adobe server that starts with version "11"
CommandBox> start cfengine=adobe@11

# Start the most recent adobe engine that matches the range
CommandBox> start cfengine="adobe@>9.0 <=11"
```

Engines are downloaded and stored in your CommandBox artifacts folder. You can view your engines and clear them using the standard artifacts commands:

```bash
CommandBox> artifacts list

# Removes all adobe servers currently in the artifacts
# These servers will need to be re-downloaded the next time they are started
CommandBox> artifacts remove adobe
```

### ColdFusion Admin settings

While Lucee asks for a password the first time running the admin, ColdFusion requires a username and password when CommandBox sets it up. The default username and password for the Adobe ColdFusion servers used are:

* Username: `admin`
* Password: `commandbox`

### WAR Support

Additionally, CommandBox can start any WAR given to it using the `WARPath` argument.

```bash
CommandBox> start WARPath=/var/www/myExplodedWAR
```

### Custom Engines

The `cfengine` parameter can accept any valid CommandBox endpoint ID. That means it can be an HTTP URL, a Git repo, a local folder path to your company's network share, or a custom ForgeBox entry you've created.  As long as that endpoint resolves to a package that contains these files, you're good:

1. `box.json`
2. `Engine.[zip|war]` (file name doesn't matter)

CommandBox will download the package, unzip it and use the WAR/zip file as the engine for your app.

Normally, the artifacts cache isn't used for non-ForgeBox packages, but CommandBox will only download the engine once per server and then assume the file hasn't changed.  You will need to forget the server to trigger a new download. 

Here's an example of starting up a web server using a direct download link to a package containing a WAR file:

```bash
CommandBox> start cfengine=http://downloads.ortussolutions.com/adobe/coldfusion/9.0.2/cf-engine-9.0.2.zip
```

### server.json Configuration

You can set the `cfengine` and other related configuration options in your `server.json` to use them every time you start your app.

```bash
CommandBox> server set app.cfengine=adobe
CommandBox> server set app.WARPath=/var/www/my-app
```

These commands would create the following `server.json`

```json
{
    "app":{
        "cfengine":"adobe",
        "WARPath":"/var/www/my-app"
    }
}
```

Just a reminder that starting a server with any command line arguments will save the arguments to your `server.json` by default.

```bash
CommandBox> start cfengine=adobe@9
```

This command would add `adobe@9` to your `server.json`.  If this is not what you want, you can append `saveSettings=false` or even `--!saveSettings` when you start your server and CommandBox will not save the arguments you specify to your `server.json`.

## OS Integration

You can start as many embedded server instances as you want.  Each running server will add a little green "Ortus" icon in your system tray with the logo of your currently running engine.  Right click on it for options:

* Stop Server
* Open Browser
* Open Admin

 <img src="../images/embedded_server/system_tray_server_icons.png" alt="System Tray Server Icons">

## Full  Control

CommandBox's embedded server does not require any prior installations of any CFML engine to work.  It does not use Apache, IIS, or Nginx.  A very lightweight Java web server called [Undertow](http://undertow.io/) is used and a context is programmatically deployed via a WAR file.

You should still have all the options you need to set up most local development servers quickly.  The web-based administrator is available to you where you can edit any setting, add data sources, CF mappings, and mail servers.  To see a list of all the parameters you can pass to the `server start` command, refer to the [CommandBox API Docs](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/commands/server/start.html) or run `server start help` command directly from the CLI.

