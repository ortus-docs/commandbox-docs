# Core Interception Points

Here is a list of all the core interception points in CommandBox that you can listen to.  Some have `interceptData` that comes along with them, while others don't.  Remember, the `interceptData` struct is passed by reference.  This means modifying any values directly in that struct will affect how processing continues afterwards inside of CommandBox where those values are used.  

Click a category for more information.

* [**CLI Lifecycle**](core/cli_lifecycle.md)
  * onCLIStart
  * onCLIExit
* [**Command Execution Lifecycle**](core/command_execution_lifecycle.md)
  * preCommand
  * postCommand
* [**Module Lifecycle**](core/module_lifecycle.md)
  * preModuleLoad
  * postModuleLoad
  * preModuleUnLoad
  * postModuleUnload
* [**Server Lifecycle**](core/server_lifecycle.md)
  * onServerStart
  * onServerInstall
  * onServerStop
* [**Error Handling**](core/error_handling.md)
  * onException
* [**Package Lifecycle**](core/package_lifecycle.md)
  * preInstall
  * postInstall
  * preUninstall
  * postUninstall
  * preVersion
  * postVersion
  * prePublish
  * postPublish
  * preUnpublish
  * postUnpublish
