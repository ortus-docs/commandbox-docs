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

## Data Structures

The `configure()` method does not accept or return any data.  Instead, the config CFC itself represents the data for configuring the module with data structures in the variables scope.  It's the job of the configure method to ensure that these data structures are created and populated.  

There is no required data, but here is the list of optional data you can set:

* `settings` - A struct of custom module settings.  These defaults can be overriden when the module is loaded with the CommandBox user config.
* `interceptors` - An array of declared interceptor structures that should be loaded in the entire application. Each interceptor structure contains `class`, and optional `properties`.
* `interceptorSettings` - A structure of settings for interceptor interactivity which includes the sub-key `customInterceptionPoints`, a list of custom interception points to add to the application wide interceptor service

Here is an example `configure()` method.  Note these data structures are placed in the variables scope.

```javascript
component{

  function configure(){
  
    // Settings for my module
    settings = {
        mySetting = 'isCool',
        settingsCanBe = [
            'complex',
            'values'
        ],
        andEven = {
            nested = {
                any = 'way'
            },
            you = 'like'
        }
    };
    
    // Declare some interceptors to listen
    interceptors = [
		{
		    class='#moduleMapping#.interceptors.TestInterceptor'
		}, 
		{
		    class='#moduleMapping#.interceptors.DoCoolThings',
		    properties={
		        coolnessFactor='max',
		        crankItToEleven=true
		        
		    }
		}
    ];
    
    // Ad-hoc interception events I will announce myself
    interceptorSettings = {
        customInterceptionPoints = 'launchInitiated,velocityAcheived,singularityAcquired'
    };
    
    // Manually map some models
    binder.map( 'foo' ).to( '#moduleMapping#.com.foo.bar' );
  
  }
}```
