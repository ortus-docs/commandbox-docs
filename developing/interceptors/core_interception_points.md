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

Announced after each module that is loaded.

## Server lifecycle

#### onServerStart

#### onServerStop

## Error handling

#### onException

## Package lifecycle

#### preInstall

#### postInstall

#### preUninstall

#### postUninstall