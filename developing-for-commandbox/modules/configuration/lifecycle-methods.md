# Lifecycle Methods

Every module follows a sequence of steps when it is loaded and unloaded. Modules are automatically loaded for you when CommandBox starts up, but here are some ways for a module to affect how it loads.

## Life-cycle Events

There are two life-cycle callback events you can declare in your `ModuleConfig.cfc`:

* `onLoad()`  - Called when the module is loaded and activated
* `onUnLoad()` - Called when the module is unloaded from memory

This gives you great hooks for you to do bootup and shutdown procedures for this specific module.

```javascript
function onLoad(){
    log.info('Module loaded successfully.' );
}

function onUnLoad(){
    log.info('Module unloaded successfully.' );
}
```

## Custom Events

The `ModuleConfig.cfc` object itself is an interceptor so you can declare all of the CLI's interception points in the configuration object and they will be registered as interceptors.

```javascript
function preCommand( interceptData ){
    // I just intercepted ALL Commands in the CLI
    log.info('The command executed is #interceptData.CommandInfo.commandString#');
}
```

