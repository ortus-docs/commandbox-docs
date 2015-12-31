# Running Other Commands

Many times when developing a command, you find the need to run another, existing command.   To do this, we have provided you with a DSL you can use to call any command, pass parameters, and even pipe commands together.  

## The DSL

The DSL is a sequence of chained methods that will always start with `command()` and end with `.run()`.  The `run` method tells the DSL that you are finished chaining methods and that the command should be executed. Here is the simplest possible example:

```javascript
command( 'version' )
    .run();
```

This runs the `version` command and the output will be flushed to the console.


Here are all the possible DSL methods that we'll unpack below:

```javascript
command( ... )
    .params( ... )
    .flags( ... )
    .append( ... )
    .overwrite( ... )
    .run( ... );
```

## command()

This is required to be the first method you call.  It creates an instance of the `CommandDSL` class and returns it.  It accepts a single parameter called `name` which is the name of the command you wish to run. 

```javascript
command( 'info' )
    .run();
```

Type the name exactly as you would in the shell including the namespace, if applicable.  

```javascript
command( 'server start' )
    .run();
```