---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/Bw6M3PI3e5HgLVcKZz0G/developing-for-commandbox/interceptors/core-interception-points/module-lifecycle
---

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
* `moduleConfig` - Struct representing the configuration data for the module. &#x20;

## preModuleUnLoad

Announced before each module that is unloaded.

**interceptData**

* `moduleName` - Name of the module

## postModuleUnload

Announced after each module that is unloaded.

**interceptData**

* `moduleName` - Name of the module
