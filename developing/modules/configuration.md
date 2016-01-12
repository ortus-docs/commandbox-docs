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

