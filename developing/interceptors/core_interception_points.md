# Core Interception Points

Here is a list of all the core interception points in CommandBox that you can listen to.  Some have `interceptData` that comes along with them, while others don't.  Remember, the `interceptData` struct is passed by reference.  This means modifying any values directly in that struct will affect how processing continues afterwards inside of CommandBox where those values are used.  

## CLI lifecycle

#### onCLIStart

Announced when shell first starts, but before any commands are run or output has been flushed to the console.  Receives the following interceptData

* `shellType` - The string `interactive` if starting the interactive shell, or `command` if running a one-off command and exiting
* `args` - An array of arguments provided from the OS when `box` was executed.
* `banner` - A string containing the CommandBox banner text that displays when in interactive mode.

#### onCLIExit

test

## Command execution lifecycle

#### preCommand

#### postCommand

## Module lifecycle

#### preModuleLoad

#### postModuleLoad

#### preModuleUnLoad

#### postModuleUnload

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