# Core Interception Points

Here is a list of all the core interception points in CommandBox that you can listen to.  Some have `interceptData` that comes along with them, while others don't.  Remember, the `interceptData` struct is passed by reference.  This means modifying any values directly in that struct will affect how processing continues afterwards inside of CommandBox where those values are used.  


## Server lifecycle

#### onServerStart

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
 
#### onServerStop

Announced before a server stop.

**interceptData**

* `serverInfo` - Same as `onServerStart` above

## Error handling

#### onException

Announced any time an unhandled exception is thrown.  

**interceptData**

* `exception` - Error struct  

## Package lifecycle

#### preInstall

Announced prior to installing a package. If a package has additional dependencies to install, each of them will fire this interception point.

**interceptData**

* `installArgs` - Struct containing the following keys used in installation of the package.
  * `ID` - The ID of the package to install
  * `directory` - Directory to install to.  May be null, if none supplied.
  * `save` - Flag to save box.json dependency
  * `saveDev` - Flag to save box.json dev dependency
  * `production` - Flag to perform a production install
  * `currentWorkingDirectory` - Original working directory that requested installation
  * `verbose` - Flag to print verbose output
  * `force` - Flag to force installation
  * `packagePathRequestingInstallation` - Path to package requesting installing.  This climbs the folders structure for nested dependencies.

#### postInstall

Announced after an installation is complete.  If a package has additional dependencies to install, each of them will fire this interception point.

**interceptData**

* `installArgs` - Same as `preInstall` above

#### preUninstall

Announced before the uninstallation of a package.

**interceptData**

* `uninstallArgs` - Struct containing the following keys used in removal of the package
  * `ID` - ID of the package to uninstall
  * `directory` - The directory to be uninstalled from (used to find box.json)
  * `save` - Whether to update box.json
  * `currentWorkingDirectory` - Path to package requesting removal .  This climbs the folders structure for nested dependencies.

#### postUninstall

Announced after the uninstallation of a package.

**interceptData**

* `uninstallArgs` - Same as `preUninstall` above
 


