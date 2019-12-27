# Running Other Commands

Many times when developing a task, you find the need to run another, existing command. To do this, we have provided you with a DSL you can use to call any command, pass parameters, and even pipe commands together.

## The DSL

The DSL is a sequence of chained methods that will always start with `command()` and end with `.run()`. The `run` method tells the DSL that you are finished chaining methods and that the command should be executed. Here is the simplest possible example:

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

## command\(\)

This is required to be the first method you call. It creates an instance of the `CommandDSL` class and returns it. It accepts a single parameter called `name` which is the name of the command you wish to run. Type the name exactly as you would in the shell including the namespace, if applicable.

```javascript
command( 'info' )
    .run();

command( 'server start' )
    .run();
```

## params\(\)

This method is used to pass parameters to your command. You can pass named or positional parameters to this method, and they will be pass along to the command in the same fashion. There is no need to escape parameter values like you would when running a command manually from the shell.

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

## flags\(\)

Just like when running a command manually, flags are an optional shortcut for specifying boolean parameters. Pass in each flag as a separate argument. It is not necessary to include the `--` prior to the value, but it will still work.

```javascript
command( "install" )
    .params( 'coldbox' )
    .flags( 'force', '!save' )
    .run();
```

## append\(\) and overwrite\(\)

You may redirect the output of a command to a file \(normally accomplished by `>` and `>>`\) by chaining the `append()` or `overwrite()` methods. These are mutually exclusive.

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

## inWorkingDirectory\(\)

Control the working directory that the command runs in if you don't want it to be the current working directory of the shell.

```javascript
command( "ls" )
.inWorkingDirectory( 'C:/' )
.run();
```

## pipe\(\)

Piping is a very powerful way to combine multiple commands and is accomplished via the `pipe` method. This method expects to receive another `CommandDSL` instance. You do not need to call `run()` on the nested command. This example is the equivalent to `echo "hello\nworld" | grep lo`.

```javascript
command( "echo" )
    .params( "hello#chr( 10) #world" )
    .pipe( 
        command( "grep" )
        .params( "lo" )
    )
    .run();
```

You can have more than one `pipe()` method. Each piped command will be called in order, receiving the output from the previous one.

```javascript
command( "cat" )
    .params( "myFile.txt" )
    .pipe( 
        command( "grep" )
        .params( "searchString" )
    )
    .pipe( 
        command( "sed" )
        .params( "s/find/replace/g" )
    )
    .pipe( 
        command( "more" )
    )
    .run();
```

The above is the equivalent of

```bash
cat myFile.txt | grep searchString | sed s/find/replace/g | more
```

## run\(\)

Your DSL should always end with a `run` method. This executes the command. By default, the output will be sent to the console, however you can capture it by specifying `returnOutput` as `true`.

```javascript
var output = command( "echo" )
      .params( "My name is Brad" )
      .run( returnOutput=true );

// You can optinally strip any ANSi formatting too
output = print.unANSI( output );
```

If you want to help debug the exact command that is being passed along to the shell for executing, set the `echo` parameter to `true` and the command will be echoed out prior to execution. The echoed text is not part of what gets returned or piped.

```javascript
command( "version" )
    .run( echo=true );
```

You may want to manually pipe data into the command \(which is the same as passing it as the first parameter. Do so with the `piped` parameter to the `run` method.

```javascript
command( "touch" )
    .run( piped='myFile' );
```

If you try to pass a shell expansion into a command, it won't work since the CommandDSL escapes all your special characters. This example doesn't work because the special characters are escaped. So the exact text is printed out and it's not possible to have it evaluated.

```javascript
command( 'echo' )
  .params( '${ os.name }' )
  .run();
------------------------------------------
Output: ${ os.name }
```

You can ask the CommandDSL to treat your parameters as 'raw' so they are just passed along. This allows them to include system setting expansions and CommandBox backtick expressions. Make sure that you escape any special chars yourself in this mode just like you would if typing the parameters from the shell.

```javascript
command( 'echo' )
  .params( '${ os.name }' )
  .run( rawParams=true );
------------------------------------------
Output: Windows 7
```

