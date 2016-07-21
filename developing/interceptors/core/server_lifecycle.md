# Server Lifecycle

## onServerStart

Announced before a server start.  Use this to modify the settings for the server before it starts.

**interceptData**

* `serverInfo` - A struct with the following keys used in starting the server
  * `name` - The name of the server
  * `webroot` - The path to the web root
  * `debug` - Whether  to start Runwar in debug mode
  * `openbrowser` - Flag to open web browser upon start
  * `host` - The hostname to bind the server to
  * `port` - The HTTP port
  * `stopsocket` - The socket to listen for stop connections
  * `webConfigDir` - Path to the Lucee web context
  * `serverConfigDir` - Path to the Lucee server context
  * `libDirs` -  List of additional lib paths
  * `trayIcon` - path to .png file for tray icon
  * `webXML` - Path to web.com file
  * `SSLEnable` - Enable HTTPS flag
  * `HTTPEnable` - Enable HTTP flag
  * `SSLPort` - HTTPS port
  * `SSLCert` - SSL Certificate
  * `SSLKey` -SSL Key 
  * `SSLKeyPass` - SSL Key passphrase
  * `rewritesEnable` - Enable URL rewrites
  * `rewritesConfig` - Path to custom Tuckey rewrite config file
  * `heapSize` - Max heap size in Megabytes
  * `directoryBrowsing` - Enable directory browsing
  * `JVMargs` - Additional JVM args to use when starting the server
  * `runwarArgs` - Additional Runwar options to use when starting the server
  * `logdir` - Path to directory for server logs
 
## onServerInstall

Announced when a server is starting and the `cfengine` is being installed.  This gives you a chance to influence how the server is installed or to modify default settings before the server process actually starts.  This is not announced for servers using a `WARPath` setting.  It is announced every time a server is started, but you can use the `isntallDetails.initialInstall` flag to determine if this is the first time the engine is being installed for one-time tasks.

**interceptData**

* `serverInfo` - Same as `onServerStart` above
* `installDetails` A struct with the following keys:
  * `internal` - True if using the embedded jars from the CLI
  * `version` - The version of the cfengine that was installed
  * `installDir` - The folder where the server is installed to
  * `intialInstall` - True if this is the first time the engine was installed

## onServerStop

Announced before a server stop.

**interceptData**

* `serverInfo` - Same as `onServerStart` above
