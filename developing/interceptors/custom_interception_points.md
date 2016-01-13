# Custom Interception Points

You have the ability to create your own interception points on top of the core ones in CommandBox.  This can be handy to employ your own event listener model within a module-- which could be comprised of multiple models and commands.  Interceptors that listen to custom interception points work the exact same as interceptors that listen to core interception points.  In fact, the same interceptor file can listen to both.

## Register 

To register your custom interception point with InterceptorService, place the following config in your module's `ModuleConfig.cfc`:

## Accounce