# Core Interception Points

Here is a list of all the core interception points in CommandBox that you can listen to.  Some have `interceptData` that comes along with them, while others don't.  Remember, the `interceptData` struct is passed by reference.  This means modifying any values directly in that struct will affect how processing continues afterwards inside of CommandBox where those values are used.  

Click a category for more information.

* [**CLI Lifecycle**](/developing/interceptors/core/cli_lifecycle.md)
  * onCLIStart
  * onCLIExit
* [**Command Execution Lifecycle**](/developing/interceptors/core/command_execution_lifecycle.md)
  * preCommand
  * postCommand
* [**Module Lifecycle**](/developing/interceptors/core/module_lifecycle.md)
  * preModuleLoad
  * postModuleLoad
  * preModuleUnLoad
  * postModuleUnload
* [**Server Lifecycle**](/developing/interceptors/core/server_lifecycle.md)
  * onServerStart
  * onServerStop
* [**Error Handling**](/developing/interceptors/core/error_handling.md)
  * onException
  * preInstall
  * postInstall
  * preUninstall
  * postUninstall
