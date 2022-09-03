# Configuration

The configuration for a module is contained within in the `ModuleConfig.cfc` that lives in the root folder. Here's an overview of the options for configuring your module.

```javascript
component{
    // Module Properties
    this.autoMapModels = true;
    this.modelNamespace = "test";
    this.cfmapping = "test";
    this.dependencies = [ "otherModule", "coolModule" ];

    function configure(){

        // Settings for my module
        settings = {
            mySetting = 'isCool',
            settingsCanBe = [
                'complex',
                'values'
            ]
        };

        // Declare some interceptors to listen
        interceptors = [
            {
                class='#moduleMapping#.interceptors.TestInterceptor'
            }
        ];

        // Ad-hoc interception events I will announce myself
        interceptorSettings = {
            customInterceptionPoints = ''
        };

        // Manually map some models
        binder.map( 'foo' ).to( '#moduleMapping#.com.foo.bar' );

    }

    // Runs when module is loaded
    function onLoad(){
        log.info('Module loaded successfully.' );
    }

    // Runs when module is unloaded
    function onUnLoad(){
        log.info('Module unloaded successfully.' );
    }

    // An interceptor that listens for every command that's run.
    function preCommand( interceptData ){
        // I just intercepted ALL Commands in the CLI
        log.info('The command executed is #interceptData.CommandInfo.commandString#');
    }    

}
```
