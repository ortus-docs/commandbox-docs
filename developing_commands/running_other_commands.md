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

This is required to be the first method you call.  It creates an instance of the `CommandDSL` class and returns it.  It accepts a single parameter called `name` which is the name of the command you wish to run.  Type the name exactly as you would in the shell including the namespace, if applicable.  

```javascript
command( 'info' )
    .run();

command( 'server start' )
    .run();
```

## params()
 
This method is used to pass parameters to your command.  You can pass named or positional parameters to this method, and they will be pass along to the command in the same fashion.  There is no need to escape parameter values like you would when running a command manually from the shell.
 
### Named parameters

```javascript
command( 'cp' )
    .params( path='/my/path', newPath='/my/new/path' )
    .run();
```
### Positional parameters

```javascript
command( 'cp' )
    .params( '/my/path', '/my/new/path' )
    .run();
```

## flags()

Just like when running a command manually, flags are an optional shortcut for specifying boolean parameters.  Pass in each flag as a separate argument.  It is not necessary to include the `--` prior to the value, but it will still work.  

```javascript
command( "install" )
    .params( 'coldbox' )
    .flags( 'force', '!save' )
    .run();
```

## append() and overwrite()

You may redirect the output of a command to a file (normally accomplished by `>` and `>>`) by chaining the `append()` or `overwrite()` methods.  These are mutually exclusive.


```javascript
command( "cat" )
    .params( "myFile.txt" )
    .append( "myOtherFile.txt" )
    .run();
    
command( "echo" )
    .params( "Your new file contents" )
    .overwrite( "myFile.txt" )
    .run();
```

## pipe()

Piping is a very powerful way to combine multiple commands and is accomplished via the `pipe` method.  This method expects to receive another `CommandDSL` instance.  You do not need to call `run()` on the nested command.  This example is the equivalent to `echo "hello\nworld" | grep lo`.

```javascript
command( "echo" )
    .params( "hello#chr( 10) #world" )
    .pipe( 
        command( "grep" )
        .params( "lo" )
    )
    .run();
```

You can have more than one `pipe()` method.

