# Module Configuration

The configuration for a module is contained within in the `ModuleConfig.cfc` that lives in the root folder.  Here's an overview of the options for configuring your module.

```javascript
component{
    // Module Properties
    this.autoMapModels = true;
    this.modelNamespace = "test";
    this.cfmapping = "test";
    this.dependencies = [ "otherModule", "coolModule" ];

  function configure(){
  
  }
}```

