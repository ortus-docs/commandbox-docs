# Shell integration

Your command might need to get information about its environment or perhaps proxy to other commands. Here is a handful of useful methods available to all commands.

## getCWD\(\)

This method will return the Current Working Directory that the user has changed to via the `cd` command. The path will be expanded and fully qualified.

## shell.clearScreen\(\)

This method on the shell object will clear all text off the screen and redraw the CommandBox prompt.

## shell.getTermWidth\(\)

This shell method returns the number of characters wide that the terminal is. Can be useful for outputting long lines and making sure they won't wrap.

## shell.getTermHeight\(\)

This shell method returns the number of characters tall the terminal is. Can be useful for [outputting ASCII art](https://github.com/bdw429s/CommandBox-Image-To-ASCII).

## runCommand\(\)

> **Warning** runCommand\(\) is now deprecated. Please use the [Command\(\) DSL](running-other-commands.md) instead.

`runCommand()` will run another command from the shell inline and wait for it to complete. Any output from the command you run will be sent to the console. You must pass the command and any parameters in **exactly** as you would enter in the interactive shell which includes escaping any special characters.

```javascript
runCommand( 'echo "Greetings planet"' );
```

If your passing through a value that may or may not need escaping, there is a method in the parser that will help you.

```javascript
runCommand( 'echo "#getInstance( 'parser' ).escapeArg( untrustedVariable )#"' );
```

By default, commands run via `runCommand()` will send their output to the console. It is possible to capture that output for your own purposes.

```javascript
// Run echo and capture its output
var result =  runCommand( command='echo hello', returnOutput=true );
// Force it to echo in green text
print.green( result ).toConsole();
```

> **Note** You won't be able to capture any output that's already flushed directly to the console.

