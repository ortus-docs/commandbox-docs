# Threading/Async

The CommandBox CLI is implemented as a single long request in the underlying Lucee server. Due to this it is required to make a unique thread name every call to the task or else an error can occur. The following is an example you can use for running threads.

```javascript
var threadName = createGUID();
cfthread( action="run" name=threadName) {
  //Thread body
}
cfthread( action="terminate" name=threadName);
```

## AsyncManager

CommandBox bundles WireBox and therefore has access to the `AsyncManager` library that ColdBox MVC has.  It can be used to create async code and even scheduled tasks.  CommandBox uses the AsyncManager internally for timed screen redraws.

You can access the `AsyncManager` class in the variables scope or call the `async()` method of any Task Runner or custom command. &#x20;

```javascript
// Parallel Executions
async().all(
    () => hyper.post( "/somewhere" ),
    () => hyper.post( "/somewhereElse" ),
    () => hyper.post( "/another" )
).then( (results)=> logResults( results ) );
```

You can find the full documentation here:

{% embed url="https://coldbox.ortusbooks.com/digging-deeper/promises-async-programming" %}

And the API docs here:

{% embed url="https://apidocs.ortussolutions.com/coldbox/current/index.html?coldbox/system/async/AsyncManager.html" %}

