# BaseTask Super Class

Much of the functionality available to you in task runners comes via inheritance from the super class that all tasks extend.  Even if you don't have an `extends` attribute, CommandBox uses the power of WireBox virtual inheritance to apply the super class. &#x20;

```
─┬ commandbox.system.BaseCommand - Base task for all custom commands
 └─┬ commandbox.system.BaseTask - base task for all task runners
   └── task - Your custom task runner
```

The see most up-to-date list of all methods and properties from the base classes, check the API docs:

{% embed url="https://s3.amazonaws.com/apidocs.ortussolutions.com/commandbox-core/current/index.html?commandbox/system/BaseTask.html" %}
BaseTask
{% endembed %}

{% embed url="https://s3.amazonaws.com/apidocs.ortussolutions.com/commandbox-core/current/index.html?commandbox/system/BaseCommand.html" %}
BaseCommand
{% endembed %}

## Properties

Here is an over view of common methods available to every task from the base classes

* `wirebox` - WireBox injector
* `CR` - carriage return ( `char(10)` )
* `formatterUtil` - [Formatter Utility](https://s3.amazonaws.com/apidocs.ortussolutions.com/commandbox-core/current/index.html?commandbox/system/util/Formatter.html)
* `fileSystemUtil` - [File System Utility](https://s3.amazonaws.com/apidocs.ortussolutions.com/commandbox-core/current/index.html?commandbox/system/util/FileSystem.html)
* `shell` - CLI [Shell](https://s3.amazonaws.com/apidocs.ortussolutions.com/commandbox-core/current/index.html?commandbox/system/Shell.html) class
* `print` - [Print helper](task-output/)
* `logBox` - LogBox factory
* `logger` - LogBox logger named after this CFC
* `parser` - CLI [Parser](https://s3.amazonaws.com/apidocs.ortussolutions.com/commandbox-core/current/index.html?commandbox/system/util/Parser.html) class
* `configService` - [Config Setting Service](https://s3.amazonaws.com/apidocs.ortussolutions.com/commandbox-core/current/index.html?commandbox/system/services/ConfigService.html)
* `SystemSettings` - [System Setting](https://s3.amazonaws.com/apidocs.ortussolutions.com/commandbox-core/current/index.html?commandbox/system/util/SystemSettings.html) helper
* `job` - [Interactive Job](interactive-jobs.md)
* `thisThread` - A reference to the current running `java.lang.Thread`
* `asyncManager` - WireBox's [AsyncManager ](https://coldbox.ortusbooks.com/digging-deeper/promises-async-programming)class

## Methods

Here is an over view of common methods available to every task from the base classes

```javascript
// Returns the AsyncManager class
async()

// Convenience method for getting stuff from WireBox
getInstance( name, dsl, initArguments={}, targetObject='' )

// Retuns current exit code
getExitCode()

// Sets exit code to be returned when task completes
setExitCode( required numeric exitCode )

// Returns the current working directory of the shell
getCWD()

// ask the user a question and wait for response
ask( message, string mask='', string defaultResponse='', keepHistory=false, highlight=true, complete=false )

// Wait until the user's next keystroke amd return the char code
waitForKey( message='' )

// Ask the user a question looking for a yes/no response and return a boolean
confirm( required message )

// Intiator for multiselect DSL. (Check "task interactiviy" page in docs)
multiSelect()

// Run another command by name.
runCommand( required command, returnOutput=false )

// Intiator for Command DSL. (Check "running other commands" page in docs)
command( required name )

// Intiator for directory watcher DSL.  (Check "Watchers" page in docs)
watch()

// This resolves an absolute or relative path using the rules of the operating system and CLI.
resolvePath( required string path, basePath=shell.pwd() )

// Intiator for globber DSL (check "Using file globs" page in docs)
globber( pattern='' )

// Intiator for PropertyFile DSL (check "property files" page in docs)
propertyFile( propertyFilePath='' )

// Report error in your task. Raises an exception that will not print the stack trace
error( required message, detail='', clearPrintBuffer=false, exitCode=1 )

// Returns true if current exit code is not 0.
hasError()

// Open a file or folder externally in the default editor for the user.
openPath( path )

// Open a URL in the user's browser
openURL( theURL, browser='' )

// Set a CommandBox environment variable
setSystemSetting( required string key, string value )

// Retrieve a Java System property or env value by name.
getSystemSetting( required string key, defaultValue )

// Retrieve a Java System property by name.
getSystemProperty( required string key, defaultValue )

// Retrieve an env value by name.
getEnv( required string key, defaultValue )

// Call this method periodically in a long-running task to check and see
// if the user has hit Ctrl-C.  This method will throw an UserInterruptException
// which you should not catch.  It will unroll the stack all the way back to the shell
checkInterrupted( thisThread=variables.thisThread )

// Loads up Java classes into the class loader that loaded the CLI for immediate use.
// (Check "Loading Ad hoc Jars" page in docs)
classLoad( paths )

// Get the current java.lang.Thread object
getCurrentThread()

// Get the current thread name
getThreadName()

```
