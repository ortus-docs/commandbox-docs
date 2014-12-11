# Error handling

When a command throws an unhandled exception, any output in its print buffer will be flushed to the screen.  Then the error message accompanied by a tag stack will be output to the screen and the user will be returned to the prompt.

If an unexpected error happens inside a command that is non-recoverable, do not attempt to try/catch it unless you can improve the error message to something more useful.  Generally speaking, just let the error bubble up and be handled by CommandBox for consistency and simplicity.

## Controlled Errors

If there are expected situations such as a file not existing, that you know might go wrong, we wholeheartedly recommend checking for these situations and using the `error()` method to alert the user.  

Errors returned from the `error()` method will not contain any stack traces, etc.  A command can emit more than one error.  This might be useful if performance several types of validation against the command's parameters.

```javascript
error( "I don't like your tone of voice" );
error( "I also can't believe you wore those socks with brown pants" );
```

You can check to see if errors have been encountered with the `hasError()` method.  This can be handy after performing a series of validation checks and then looking to see if any of them failed so you can abort execution with a `return` statement.

```javascript
function run() {
    if( condition1 ) { error( 'message 1' ); }
    if( condition2 ) { error( 'message 2' ); }
    if( condition3 ) { error( 'message 3' ); }

    // Bail if stuff went wrong
    if( hasError() ) {
        return;
    }

    // Otherwise Continue execution
}

```

If you don't wish to continue after an error, simply return on one line:

```javascript
function run() {
    if( condition1 ) {
        return error( 'message 1' );
    }
}

```



