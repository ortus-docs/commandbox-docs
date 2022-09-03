# Loading Ad-hoc Modules

If you have a module installed to a folder that you wish to load into the CLI and use on-the-fly during a task runner, you can do so like this:

```
loadModule( 'build/modules/myUtils' );
```

That code will load the `myUtils` module right there into the core CLI.  Note, the module needs to be compatible with CommandBox.  All models, interceptors, or commands inside the module will instantly be available in the CLI.&#x20;

This means your task runners can rely on other functionality in the form of modules which are easily distributed and shared.  While you could manually install the module globally into the CLI, this method is more flexible and ad-hoc as the module is just temporarily loaded.

### Considerations

* Modules aren't unloaded for you. You can do so via `ModuleService.unload( 'myUtils' )` if you like.
* If a module of that name is already loaded, nothing happens.  That means if you modify the module, you'll need to `reload` the shell to pick up those changes.
* The path to the module can be absolute, but if relative, it will be resolved relative to the task runner CFC, not the current working directory of the shell.

### Loading Multiple Modules

A task runner with an entire folder of modules to load may run into timing issues where one module requires another to be loaded before it can load.  To handle this, instead of loading each module one at a time with `loadModule()` you can load an entire array of modules at ones with `loadModules()`.&#x20;

1. First all modules in the array are registered with the `ModuleService`.
2. Then each module is activated

This function takes an array of absolute module paths to load.

```javascript
loadModules( [
  "C:/path/to/module1",
  "C:/path/to/module2",
  "C:/path/to/module3"
] );
```

You can easily drive this function with `dire`c`toryList()` like so:

```javascript
loadModules(
    directoryList( path=resolvePath( 'modules/' ), type='dir' )
);
```
