# ModuleConfig.cfc Configure() Method

The `configure()` method will be run before a module is loaded.  

## Injected Variables

The following variables will be created for you in the variables scope of the `ModuleConfig.cfc`.

* `shell` - The shell object (kind of the core CommandBox controller)
* `moduleMapping` - The component mapping to the module
* `modulePath` - The physical path to the module
* `logBox` - The LogBox instance
* `log` - A named logger, ready to log!
* `wirebox` - Your friendly neighborhood DI engine
* `binder` - The WireBox binder, handy for mapping models manually

# Data Structures

The `configure()` method does not accept or return any data.  Instead, the config CFC itself represents the data for configuring the module with data structures in the variables scope.  It's the job of the configure method to ensure that these data structures are created and populated.  

There is no required data, but here is the list of optional data you can set:

