# Error handling

When a command throws an unhandled exception, any output in its print buffer will be flushed to the screen. Then the error message accompanied by a tag stack will be output to the screen and the user will be returned to the prompt.

If an unexpected error happens inside a command that is non-recoverable, do not attempt to try/catch it unless you can improve the error message to something more useful. Generally speaking, just let the error bubble up and be handled by CommandBox for consistency and simplicity.

## Controlled Errors

If there are expected situations such as a file not existing, that you know might go wrong, we wholeheartedly recommend checking for these situations and using the `error()` method to alert the user.

Errors returned from the `error()` method will not contain any stack traces, etc.

```javascript
error( "I don't like your tone of voice" );
```

You can set a detail and an error code as well.  The error code will be used as the exit code for the command.

```javascript
error( "We're sorry, but happy hour ended 20 minutes ago.", "Sux 2B U", 123 );
```

The CFML exception that is thrown from the `error()` method will contain the exit code in the `errorcode` part of the cfcatch struct.  The Exit code is also available as a system setting called `exitCode`.  

```bash
> !git status
fatal: not a git repository (or any of the parent directories): .git
Command returned failing exit code [128]

> echo ${exitCode}
128
```

## Verbose Errors in the Shell

When an unhandled exception happens in the shell, only the tag context is shown.  You can enable full stack trace output like so:

```bash
config set verboseErrors=true
```





