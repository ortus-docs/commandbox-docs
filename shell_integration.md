# Shell integration

Your command might need to get information about its environment or perhaps proxy to other commands.  Here is a handful of useful methods available to all commands.

## runCommand()

`runCommand()` will run another command from the shell inline and wait for it to complete.  Any output from the command you run will be sent to the console.  You must pass the command and any parameters in **exactly** as you would enter in the interactive shell which includes escaping any special characters.

```javascript
var result =  runCommand( command='echo hello', returnOutput=true );
print.green( result ).toConsole();
```

## getCWD()


## shell.clearScreen()


## shell.getTermWidth()


## shell.getTermHeight()


