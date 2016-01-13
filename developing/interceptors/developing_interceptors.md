# Developing Interceptors

Interceptors are a powerful event-driven model inside CommandBox that allows you to listen to broadcasts from within the core code to respond to events and even influence execution.  Interceptors are packaged within CommandBox Modules and can be for your own use, just debugging, or to share with the world.

To create a simple interceptor, re-create the simple folder structure below inside your CommandBox installation directory.

```
~/.CommandBox/cfm/modules
+-- myModule/
    |-- ModuleConfig.cfc
    +-- interceptors/
        +-- MyInterceptor.cfc
```

## Creating An Interceptor

An interceptor is a CFC that has one or more methods whose names match the name of an interception point that is broadcast.  Interceptors are packaged within CommandBox modules.  When the module is loaded, the InterceptorService will register your interceptor and take care of calling it when necessary.

Here is an example of an interceptor.  It listens to the `postCommand` event and upper cases all output from the command before it is returned to the console.  It also listens to `onException` to perform some additional error processing.

**`MyInterceptor.cfc`**
```javascript
component {

    // This runs after every command execution
    function postCommand( interceptData ) {
        // Overwrite the results with an upper case version of itself
        interceptData.results = ucase( interceptData.results );
    }

    // This runs after every error
    function onException( interceptData ) {
        // Write the last exception to a file
        fileWrite( '/commandbox-home/logs/lastError.txt', serializeJSON( interceptData.exception ) );
    }
    
}
```

## Registering An Interceptor

Now that we have our interceptor above, how do we register it?  This code in our module's config file registers the interceptor so it will listen to the announcements it has declared.


**`ModuleConfig.cfc`**
```javascript
component{
    function configure(){
        
        // Declare our interceptors to listen
        interceptors = [
        	{ class='#moduleMapping#.interceptors.MyInterceptor' }
        ];
 
    }
}
```