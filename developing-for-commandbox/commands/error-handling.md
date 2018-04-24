# Error handling

When a command throws an unhandled exception, any output in its print buffer will be flushed to the screen. Then the error message accompanied by a tag stack will be output to the screen and the user will be returned to the prompt.

If an unexpected error happens inside a command that is non-recoverable, do not attempt to try/catch it unless you can improve the error message to something more useful. Generally speaking, just let the error bubble up and be handled by CommandBox for consistency and simplicity.

## Controlled Errors

If there are expected situations such as a file not existing, that you know might go wrong, we wholeheartedly recommend checking for these situations and using the `error()` method to alert the user.

Errors returned from the `error()` method will not contain any stack traces, etc.

```javascript
error( "I don't like your tone of voice" );
```

