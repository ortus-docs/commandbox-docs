# Server Lifecycle

## preServerStart

Announced before a server starts. This fires after `server.json` has been located but before any configuration is resolved. Use this to override any user inputs, influence how the server's details are resolved, or to modify things like hostname before ports are bound.

**interceptData**

* `serverDetails` - A struct with the following keys used in starting the server
  * `defaultName` - The name of the server
  * `defaultwebroot` - The web root of the server
  * `defaultServerConfigFile` - The location of the server.json \(May not exist yet\)
  * `serverJSON` - The parsed contents of the JSON file
  * `serverInfo` - The serverInfo Struct \(see below\)
  * `serverIsNew` - A boolean whether this server has been started before.
* `serverProps` - A struct with the parameters passed to the start command from the CLI.  Omitted params will not be present.
  * See the help for the `server start` command to see the current list of parameters.

## onServerStart

Announced as a server is starting after the configuration values have been resolved, but prior to the actual server starts. Use this to modify the settings for the server before it starts.

**interceptData**

* `serverInfo` - A struct with the following keys used in starting the server
  * `name` - The name of the server
  * `webroot` - The path to the web root
  * `serverConfigFile` - The path to the server.json file \(may not exist\)
  * `trayEnable` - If tray menu is enabled
  * `customServerFolder` - Where the server's log files live. May be the same as `serverHomeDirectory`
  * `debug` - Whether  to start Runwar in debug mode
  * `trace` - Whether  to start Runwar in trace mode
  * `console` - Whether  to start server in console mode
  * `openbrowser` - Flag to open web browser upon start
  * `host` - The hostname to bind the server to
  * `port` - The HTTP port
  * `stopsocket` - The socket to listen for stop connections
  * `webConfigDir` - Path to the Lucee web context
  * `serverConfigDir` - Path to the Lucee server context
  * `libDirs` -  List of additional lib paths
  * `trayEnable` - If tray menu is enabled
  * `trayIcon` - Path to .png file for tray icon
  * `trayOptions` - Array of tray menu options
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
  * `welcomeFiles` - List of welcome files
* `serverDetails` - Same as `onServerStart` above
* `installDetails` - Same as `onInstall` below

## onServerInstall

Announced when a server is starting and the `cfengine` is being installed. This gives you a chance to influence how the server is installed or to modify default settings before the server process actually starts. This is not announced for servers using a `WARPath` setting. It is announced every time a server is started, but you can use the `installDetails.initialInstall` flag to determine if this is the first time the engine is being installed for one-time tasks.

**interceptData**

* `serverInfo` - Same as `onServerStart` above
* `installDetails` A struct with the following keys:
  * `internal` - True if using the embedded jars from the CLI
  * `enginename` - The name of the cfengine that was installed
  * `version` - The version of the cfengine that was installed
  * `installDir` - The folder where the server is installed to
  * `initialInstall` - True if this is the first time the engine was installed

## onServerStop

Announced before a server stop.

**interceptData**

* `serverInfo` - Same as `onServerStart` above

## **preServerForget**

Always fires before attempting to forget a server whether or not the forgetting is actually successful. Has access to all files and settings for the server.

**interceptData**

* `serverInfo` - Same as `onServerStart` above

## **postServerForget**

Fires after a successful server forget. If the forget fails, this will not fire.

**interceptData**

* `serverInfo` - Same as `onServerStart` above

