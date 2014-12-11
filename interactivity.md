# Interactivity

Some commands simply do some processing, output their results, and finish executing.  You may want to interact with the user or create a long-running that can be controlled by user input (See the `snake` game) 

Use these methods made available to interact with your users

## ask()

The `ask()` command will wait for the to enter any amount of text until a line break is entered.  A message must be supplied that lets the user what you'd like to receive from them.  The command will return their response in a string variable.  

```javascript
var favoriteColor = ask( 'WHAT, is your favorite color??' );

if( favoriteColor == 'red' ) {
    print.boldRedLine( 'AAAAHHHHHH!!!!' );
} else if ( favoriteColor == 'I mean blue' ) {
    print.line( 'Obscure Monty Python reference' );
}
```

##waitForKey()

If you just need a single character collected from a user, or perhaps any keystroke at all, use the `waitForKey()` method.  A message must be supplied that lets the user know what you need.  The **ASCII code** representing the character they enter will be returned in a string variable.

```javascript
var ASCIICode = waitForKey( 'Press any key, any key.' );
print.line().line( 'My magic tells me you pressed: #Chr( ASCIICode )#' );
```
## confirm()


```javascript
var favoriteColor = ask( 'WHAT, is your favorite color??' );

if( favoriteColor == 'red' ) {
    print.boldRedLine( 'AAAAHHHHHH!!!!' );
} else if ( favoriteColor == 'I mean blue' ) {
    print.line( 'Obscure Monty Python reference' );
}
```

force