# Error Handling

When a task throws an unhandled exception, any output in its print buffer will be flushed to the screen. Then the error message accompanied by a tag stack will be output to the screen and the user will be returned to the prompt.

If an unexpected error happens inside a task that is non-recoverable, do not attempt to try/catch it unless you can improve the error message to something more useful. Generally speaking, just let the error bubble up and be handled by CommandBox for consistency and simplicity.

## Controlled Errors

If there are expected situations such as a file not existing, that you know might go wrong, we wholeheartedly recommend checking for these situations and using the `error()` method to alert the user. Errors returned from the `error()` method will not contain any stack traces, etc.

```javascript
error( "I don't like your tone of voice" );
```

Or with an exit code:

```javascript
error( message="prepare to be deleted", exitCode=666 );
```

If you want your task to set a failing exit code, but you don't have a message to go with it, you can also simply return the exit code you want using the `return` keyword.

```javascript
function run() {
  if( condition ) {
    print.greenLine( 'Well that was easy' );
  } else {
    return 1;
  }
}
```
