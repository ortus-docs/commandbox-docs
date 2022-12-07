# Custom Predicates/Handlers

CommandBox bundles some custom predicates and handlers to make your life easier. &#x20;

The predicate `cf-admin()` will returns `true` if the incoming URL is to the Lucee or ColdFusion admin:&#x20;

```javascript
cf-admin() -> set-error( 404 ); 

// Is the equivalent of 
regex(pattern='^/(CFIDE/administrator|CFIDE/adminapi|CFIDE/AIR|CFIDE/appdeployment|CFIDE/cfclient|CFIDE/classes|CFIDE/componentutils|CFIDE/debug|CFIDE/images|CFIDE/orm|CFIDE/portlets|CFIDE/scheduler|CFIDE/ServerManager|CFIDE/services|CFIDE/websocket|CFIDE/wizards|lucee/admin)/.*', case-sensitive=false) -> set-error(404) 

```

The handler `block-external()` blocks any request not from localhost with a 404 response code. You can pair this with any predicate you choose.

```javascript
cf-admin() -> block-external() 

// Is the equivalent of 
cf-admin() -> ip-access-control( default-allow=false, failure-status=404, acl={ 127.*.*.* allow } ) 
```

The handler `block-cf-admin()` blocks any request to the Lucee or ColdFusion admin with a 404 response code.

```javascript
block-cf-admin() 

// Is the equivalent of 
cf-admin() -> set-error( 404 ); 
```

## Build Your Own

You can build your own predicates and handlers by compiling java classes that implement the `io.undertow.predicate.PredicateBuilder` or `io.undertow.server.handlers.builder.HandlerBuilder` interfaces.  If the proper service metadata is present in your jar's META-INF folder, undertow will automatically find and register your builders via Java's service loader. &#x20;

This isn't super difficult but involves a few moving parts.  If you're interested in writing your own handlers and predicates to use in your server rules, reach out on the mailing list and we'll walk you through it.

You can see the custom handlers and predicates documented above in our Runwar source code here:

**Custom handlers:**

{% embed url="https://github.com/Ortus-Solutions/runwar/tree/develop/src/main/java/runwar/undertow/handler" %}

**Custom Predicates:**

{% embed url="https://github.com/Ortus-Solutions/runwar/tree/develop/src/main/java/runwar/undertow/predicate" %}

