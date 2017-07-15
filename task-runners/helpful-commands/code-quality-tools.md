# Coe Quality Tools

Here's some commands to help with code quality.

## Remove Trailing Spaces

Removes trailing whitespace from the ends of your lines.

### From the CLI

```
utils remove-trailing-spaces **.cf*
```

### From CFML
```js
command( 'rts' )
    .params( '**.cf*' )
    .run();
```
