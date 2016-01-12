# ModuleConfig.cfc Lifecycle Methods

Every module follows a sequence of steps when it is loaded and unloaded.  Modules are automatically loaded for you when CommandBox starts up, but here are some ways for a module to affect how it loads. 

## Life-cycle Events

There are two life-cycle callback events you can declare in your `ModuleConfig.cfc`:

* `onLoad()`  - Called when the module is loaded and activated
* `onUnLoad()` - Called when the module is unloaded from memory

This gives you great hooks for you to do bootup and shutdown commands for this specific module. You can build a ContentBox or Wordpress like application very easily all built in to the developing platform.

```javascript
function onLoad(){
  // Register some tables in my database and activate some features
  controller.getWireBox().getInstance('MyModuleService').activate();
    log.info('Module #this.title# loaded correctly');
}

function onUnLoad(){
  // Cleanup some stuff and logit
  controller.getWireBox().getInstance('MyModuleService').shutdown();

  // Log we unloaded
  log.info('My module unloaded successfully!);
}
```

## Custom Events

Also, remember that the configuration object itself is an interceptor so you can declare all of the framework's interception points in the configuration object and they will be registered as interceptors.

```javascript
function preProcess(event, interceptData){
  // I just intercepted ALL incoming requests to the application
  log.info('The event executed is #arguments.event.getCurrentEvent()');
}
```
