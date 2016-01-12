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
