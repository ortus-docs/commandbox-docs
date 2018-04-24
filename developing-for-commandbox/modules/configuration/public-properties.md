# Public Properties

You can control how CommandBox loads your module by setting optional settings in the `this` scope.

* `this.autoMapModels` - Will automatically map all model objects under the models folder in WireBox using `@modulename` as part of the alias.
* `this.modelNamespace` - The name of the namespace to use when registering models in WireBox. Defaults to name of the module.
* `this.cfmapping` - The CF mapping that will be registered for you that points to the root of the module. Defaults to name of the module.
* `this.disabled` - You can manually disable a module from loading and registering.
* `this.dependencies` - An array of dependent module names. All dependencies will be registered and activated FIRST before the module declaring them

```javascript
component{
    // Module Properties
    this.autoMapModels = true;
    this.modelNamespace = "test";
    this.cfmapping = "test";
    this.dependencies = [ "otherModule", "coolModule" ];

  function configure(){}
}
```

