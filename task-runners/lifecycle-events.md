# Lifecycle Events

If a task runner has methods of this name, they will be executed automatically.

* **preTask** - Before any target in the task
* **postTask** - After any target in the task
* **aroundTask** - Wraps execution of any target in the task
* **pre\<targetName>** - Before a specific target (Ex: `preRun`)
* **post\<targetName>** - After a specific target (Ex: `postRun`)
* **around\<targetName>** - Wraps execution of a specific target (Ex: `aroundRun`)
* **onComplete** - Fires regardless of exit status
* **onSuccess** - Fires when task runs without failing exit code or exception
* **onFail** - Fires if exit code is failing after the action is done (always fires along with onError, but does not receive an exception object). Use this to respond generally to failures of the job.
* **onError** - fires only if an unhandled exception is thrown and receives exception object. Use this to respond to errors in the task. Does not fire for interrupted exceptions
* **onCancel** - Fires when the task is interrupted with Ctrl-C

Every event receives the following data:

* **target** - The name of the target executing
* **taskArgs** - A struct containing the arguments for the target method

The **onError** event receives this additional data:

* **exception** - The cfcatch object

The **aroundTask** and **around\<target>** events receive the this additional data:

* **invokeUDF** - A UDF to invoke to call the actual target method

A simple “around” event looks like this:

```javascript
function aroundTask() {
  return invokeUDF();
}
```

A more complex one, like this:

```javascript
function aroundTask( string target, struct taskArgs, any invokeUDF ) {

	print.line( 'aroundTask before #target#' );
	
	local.result = invokeUDF();
	
	print.line( 'aroundTask after #target#' );
	
	if( !isNull( local.result ) ) {
		return local.result;
	} 
	
}
```

It is important to _return any value received_ from the **invokeUDF()** method (may return null) in case the original target returns an exit code. Otherwise, a failing exit code returned by the target will be ignored!

If you wish to modify the incoming arguments to the target, you may modify the **taskArgs** struct, which is passed by reference. It is not necessary to pass anything into the **invokeUDF()** method call.

```javascript
function aroundTask( string target, struct taskArgs, any invokeUDF ) {
  // Override incoming parameter
  taskArgs.foo='bar';
  return invokeUDF();
}
```

Control when lifecycle method fire with these **this** scoped variables in the task CFC. These are all comma-delimited lists. When empty, they are ignored. When at least one target name is specified in “only”, then ONLY those targets will have the corresponding event fire. Any targets listed in the “except” settings, will not fire. These settings apply to the primary target being executed as well as any 'depends” targets.

```javascript
this.preTask_only='';
this.preTask_except='';
this.postTask_only='';
this.postTask_except='';
this.aroundTask_only='';
this.aroundTask_except='';
this.onComplete_only='';
this.onComplete_except='';
this.onSuccess_only='';
this.onSuccess_except='';
this.onFail_only='';
this.onFail_except='';
this.onError_only='';
this.onError_except='';
this.onCancel_only='';
this.onCancel_except='';
```

When one target depends on other targets, it makes sense for pre and post events to fire, but it doesn’t make sense for `onComplete`, `onFail`, `onSuccess`, `onError`, or `onCancel` to fire for each one since those are “final” events. As such, the following will only fire for the top level target being executed. (Keep in mind, if a “depends” target fails, the error will bubble up to the top level.

* **onError**
* **onSuccess**
* **onFail**
* **onComplete**
* **onCancel**

If an `aroundTask` and `around<target>` method are both specified, they will both execute with the task-level one (more generic) wrapped “around” the target level one (more specific).

To clarify the order of execution, here is the output from a successful task running a “run” target that has every possible method above defined.

```javascript
aroundTask prior to invokeUDF()
 preTask
  aroundRun prior to invokeUDF()
   preRun
    Task execution here
   postRun
  aroundRun after invokeUDF()
 postTask
```

ColdBox notes: this feature is modeled after the pre/post/around features of ColdBox event handlers where a **Task** is analogous to a **handler** and a **target** is analogous to an **action**. There are some differences in how CommandBox’s pre/post/around events were implemented.

* ColdBox does not allow an **aroundhandler** and **around\<action>** at the same time, but CommandBox does allow an **aroundTask** and **around\<target>** at the same time.
* ColdBox executes **pre**/**post** before and after the **around** events. CommandBox wraps the **pre**/**post** code with the **around** code. This means, for example, exceptions raised by a **preTask** or **postTask** method can be try/caught in the **aroundTask** method.
* For **around** methods, ColdBox passes a direct reference to the original action UDF to invoke and all the original arguments must be passed along by **argumentCollection**. CommandBox passes a special invoker UDF to **around** methods which does not require any arguments as it internally calls the original target method with the necessary arguments. This reduces boilerplate. In both cases, the return value of the UDF needs to be dealt with appropriately.
