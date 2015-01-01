# Command Output

Commands aren't required to output anything, but if you do, the simplest way is to simply return a string from your run() method.

```javascript
function run() {
    return 'Hi Mom!';
}
```

The recommended method is via a `print` object.  You can output ANSI-formatted text this way.  All the text you output  will be stored in a "buffer" and at the end of the command it will be output to the console, or piped into the next command if necessary;  

## Print Helper

The print object has an unlimited number of methods you can call on it since it uses onMissingMethod.  Here are the rules.

### Line Break

If the method has the word "line" in it, a new line will be added to the end of the string.  If the string is empty, you'll just output a blank line.

```javascript
print.line( 'I like Spam.' );
print.line( '' );
```

### Text Color

If the method has one of the 8 ANSI colors in it, the text will be colored.

* black
* red
* green
* yellow
* blue
* magenta
* cyan
* white

```javascript
print.magenta( 'I sound like a toner cartridge' );
print.greenText( "You wouldn't like me when I'm angry" );
print.somethingBlue( 'UHF ' );
```

### Background Color

If the method has one of the 8 ANSI colors preceded by the word "on", the background of the text will be that color.

* onBlack
* onRed
* onGreen
* onYellow
* onBlue
* onMagenta
* onCyan
* onWhite

```javascript
print.blackOnWhiteText( 'Inverse!' );
print.greenOnRedLine( "Christmas?" );
```

### Text Decoration

If any of the following words appear in the method, their decoration will be added.  Note, not all of these work on all ANSI consoles.  Blink, for instance, doesn't seem to work in Windows.

* bold
* underscored
* blinking
* reversed
* concealed

```javascript
print.boldText( "Don't make me turn this car around!" );
print.underscoredLine( "Have I made my point?" );
```

### Mix It Up

Any combination of the above is possible.  Filler words like "text" will simply be ignored so you can make your method nice and readable.  Get creative, but just don't overdo it.  No one wants their console to look like a rainbow puked on it.

```javascript
print.redOnWhiteLine( 'Ready the cannons!' );
print.boldRedOnBlueText( "Test dirt, don't wash." );
print.boldBlinkingUnderscoredBlueTextOnRedBackground( "That's just cruel" );
```

### Flush

If you have a command that takes a while to complete and you want to update the user right away, you can flush out everything in the buffer to the console with the .toConsole() method.  Note, any text flushed to the console cannot be piped to another command.


```javascript
print.Line( 'Step 1 complete' ).toConsole();
print.Line( 'Step 2 complete' ).toConsole();
print.Line( 'Step 3 complete' ).toConsole();
```

## Chaining

All the methods in the `print` object can be chained together to clean up your code.

```javascript
	print.whiteOnRedLine( 'ERROR' )
		.line()
		.redLine( message )
		.line();

```






