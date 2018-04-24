# Custom Interception Points

You have the ability to create your own interception points on top of the core ones in CommandBox. This can be handy to employ your own event listener model within a module-- which could be comprised of multiple models and commands. Interceptors that listen to custom interception points work the exact same as interceptors that listen to core interception points. In fact, the same interceptor file can listen to both.

## Register

To register your custom interception point with InterceptorService, place the following config in your module's `ModuleConfig.cfc`. This module is registering a custom interception point called `onCustomEvent`.

```javascript
component{

  function configure(){

    interceptorSettings = {
        customInterceptionPoints = 'onCustomEvent'
    };

  }
}
```

## Announce

It's up to you to decide when to announce this event and what intercept data you want to provide to it. To announce, use the `announceInterception` method in the InterceptorService. Here's a model that shows how:

```javascript
component{
    property name="InterceptorService" inject="InterceptorService";

    function doSomethingAmazing(){

        interceptorService.announceInterception(
            state='onCustomEvent',
            interceptData={
                data='foo',
                moreData='bar'
            }
        );

    }
}
```

