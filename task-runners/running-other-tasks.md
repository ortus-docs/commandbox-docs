# Running other Tasks

When developing a task, you may find the need to run another task. To do this, we have provided you with a DSL you can use to call any task. The Task DSL is very similar to the Command DSL, but designed to delegate to the `task run` command for you.

## The DSL

The DSL is a sequence of chained methods that will always start with `task()` and end with `.run()`. The `run` method tells the DSL that you are finished chaining methods and that the task should be executed. Here is the simplest possible example:

```javascript
task().run();
```

This would run a `task.cfc` in the current working directory and the output would be flushed to the console.

Here are all the possible DSL methods that we'll unpack below:

```javascript
task( ... )
    .target( ... )
    .params( ... )
    .flags( ... )
    .inWorkingDirectory( ... )
    .run( ... );
```

## task\(\)

This is required to be the first method you call. It creates an instance of the `TaskDSL` class and returns it. It accepts a single parameter called `taskFile` which is the path of the task CFC you wish to run. Just like the `task run` command, you can supply a full path or a relative path. The `.cfc` extension is also optional. If you don't pass in a task CFC name, it defaults to `task`.

```javascript
task( 'build' )
    .run();

 task( 'C:/path/to/task.cfc' )
    .run();
```

## target\(\)

Use this method to override the default task target of `run`.

```javascript
task()
    .target( '' )
    .run();

 task( 'C:/path/to/task.cfc' )
    .run();
```

## params\(\)

This method is used to pass parameters to your command. You can pass named or positional parameters to this method, and they will be pass along to the command in the same fashion. There is no need to escape parameter values like you would when running a command manually from the shell.

### Named parameters

```javascript
task( 'mytask' )
    .params( path='/my/path', newPath='/my/new/path' )
    .run();
```

### Positional parameters

```javascript
 task( 'mytask' )
    .params( '/my/path', '/my/new/path' )
    .run();
```

## flags\(\)

Just like when running a task manually, flags are an optional shortcut for specifying boolean parameters. Pass in each flag as a separate argument. It is not necessary to include the `--` prior to the value, but it will still work.

```javascript
 task( "mytask" )
    .params( 'coldbox' )
    .flags( 'force', '!save' )
    .run();
```

## inWorkingDirectory\(\)

Control the working directory that the task runs in if you don't want it to be the current working directory of the shell.

```javascript
task()
.inWorkingDirectory( 'C:/' )
.run();
```

## run\(\)

Your DSL should always end with a `run` method. This executes the task. By default, the output will be sent to the console, however you can capture it by specifying `returnOutput` as `true`.

```javascript
var output =  task()
      .params( "My name is Brad" )
      .run( returnOutput=true );

// You can optinally strip any ANSi formatting too
output = print.unANSI( output );
```

If you want to help debug the exact task that is being passed along to the shell for executing, set the `echo` parameter to `true` and the task will be echoed out prior to execution. The echoed text is not part of what gets returned.

```javascript
task()
    .run( echo=true );
```

