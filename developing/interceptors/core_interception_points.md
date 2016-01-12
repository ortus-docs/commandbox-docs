# Core Interception Points

Here is a list of all the core interception points in CommandBox that you can listen to.  Some have `interceptData` that comes along with them, while others don't.  Remember, the `interceptData` struct is passed by reference.  This means modifying any values directly in that struct will affect how processing continues afterwards inside of CommandBox where those values are used.  

## CLI lifecycle

#### onCLIStart

Announced when shell first starts, but before any commands are run or output has been flushed to the console. 

**interceptData**

* `shellType` - The string `interactive` if starting the interactive shell, or `command` if running a one-off command and exiting
* `args` - An array of arguments provided from the OS when `box` was executed.
* `banner` - A string containing the CommandBox banner text that displays when in interactive mode.

This fires every time the `reload` command runs and a fresh shell is created.

#### onCLIExit

Announced right before the shell exits and control is returned back to the OS.  This fires every time the `reload` command runs right before the shell is destroyed and re-created.

## Command execution lifecycle

#### preCommand

Announced before the execution of a command.  This fires after all command parameters have been evaluated, including expressions.  If piping the output of one command into another in a command chain, this will fire twice-- once for each command in the chain.


**interceptData**

* `commandInfo` - A struct containing the following keys about the command to execute
  * `commandString` - A string representing the command name
  * `commandReference` - The instantiated Command CFC
  * `parameters` - An array of un-parsed parameter tokens typed in the CLI
  * `closestHelpCommand` - The CFC path to the most-applicable help command. Used to generate namespace help.
* `parameterInfo` - A struct containing the following keys about the processed parameters for the command execution
  * `positionalParameters` - An array of parameter values
  * `namedParameters` - A struct of name/value pairs.  The named parameters are always what is passed to the command's `run()` method.
  * `flags` - A struct of flags that were passed in.


#### postCommand

Announced immediately after command execution is complete.  If more than one command is piped together in a command chain, this is announced after each command in the chain.


**interceptData**

* `commandInfo` - Same as `preCommand` above
* `parameterInfo` - Same as `preCommand` above
* `results` - A string that represents any output from the command that hasn't already been flushed to the console.

## Module lifecycle

#### preModuleLoad

Announced before each module that is loaded.

**interceptData**

* `moduleLocation` - Path to the module
* `moduleName` - Name of the module

#### postModuleLoad

Announced after each module that is loaded.

**interceptData**

* `moduleLocation` - Path to the module
* `moduleName` - Name of the module
* `moduleConfig` - Struct representing the configuration data for the module.  

#### preModuleUnLoad

Announced before each module that is unloaded.

**interceptData**

* `moduleName` - Name of the module

#### postModuleUnload

Announced after each module that is unloaded.

**interceptData**

* `moduleName` - Name of the module

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

## Package lifecycle

#### preInstall

#### postInstall

#### preUninstall

#### postUninstall