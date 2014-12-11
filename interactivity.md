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

>**Note** It is a known behavior that multi-byte characters such as the up arrow will only return the first byte which kind of makes them useless.  We have [logged a bug](https://github.com/jline/jline2/issues/152) in the Java project that handles the low-level shell.


## confirm()

If you want to ask the user a yes or no question, use the `confirm()` method.  Any boolean that evaluates to true or a `y` will return true.  Everything else will return false.  This allows your users to respond with what's natural to them like `yes`, `y`, `no`, `n`, `true`, or `false`.  You must pass a question into the method and you will receive a boolean back.

```javascript
if( confirm( 'Do you like Pizza? [y/n]' ) ) {
    print.greenLine( 'Good for you.' );
} else {
    print.boldRedLine( 'Heretic!!' );
}
```

## Force

Remember that while interactivity is cool, people might want to automate your commands as part of a script that runs headlessly.  Therefore you should always provide a way to skip prompts if possible.  An example is the `rm` command.  It usually confirms the deletion but can be told to stand down.

```javascript
function run( required path, Boolean force=false )  {
	if( arguments.force || confirm( "Are you sure? [y/n]" ) ) {
	    fileDelete( arguments.path );
	    print.redLine( "It's gone, baby." );
	    return;
	}
	print.redLine( "Chickened out!" );
```





