# Module Lifecycle

## preModuleLoad

Announced before each module that is loaded.

**interceptData**

* `moduleLocation` - Path to the module
* `moduleName` - Name of the module

## postModuleLoad

Announced after each module that is loaded.

**interceptData**

* `moduleLocation` - Path to the module
* `moduleName` - Name of the module
* `moduleConfig` - Struct representing the configuration data for the module.  

## preModuleUnLoad

Announced before each module that is unloaded.

**interceptData**

* `moduleName` - Name of the module

## postModuleUnload

Announced after each module that is unloaded.

**interceptData**

* `moduleName` - Name of the module

