# Task Output

Tasks aren't required to output anything, but if you do, use the handy `print` helper that lives in the `variables` scope. You can output ANSI-formatted text this way. All the text you output will be stored in a "buffer" and at the end of the task it will be output to the console, or piped into the next command if necessary;

## Print Helper

The print object has an unlimited number of methods you can call on it since it uses onMissingMethod. Here are the rules.

### Line Break

If the method has the word "line" in it, a new line will be added to the end of the string. If the string is empty or not provided, you'll just output a blank line.

```javascript
print.line( 'I like Spam.' );
print.line();
```

### Text Color

CommandBox supports 256 colors, but some terminals only support 16 or even 8.  If you use a color that the terminal doesn't support, it will be adjusted to the next closest color.  If the method has one of the names of a supported color in it, the text will be colored.  Here are the basic 16 color names:

* Black
* Maroon
* Green
* Olive
* Navy
* Magenta
* Cyan
* Silver
* Grey
* Red
* Lime
* Yellow
* Blue
* Fuchsia
* Aqua
* White

```javascript
print.magenta( 'I sound like a toner cartridge' );
print.greenText( "You wouldn't like me when I'm angry" );
print.somethingBlue( 'UHF ' );
```

To view all the color names run the `system-colors` command.

```text
print.MistyRose3( 'Fancy colors' );
```

### Background Color

If the method has a valid color name preceded by the word "on", the background of the text will be that color.

* onBlack
* onRed
* onGreen
* onYellow
* onBlue
* onMagenta
* onCyan
* onWhite
* etc...

```javascript
print.blackOnWhiteText( 'Inverse!' );
print.greenOnRedLine( "Christmas?" );
```

### Color by Numbers

When you run the system-colors command, you'll see that each of the 256 colors have a number.  You can reference a color like so:

```text
print.color221( 'I will print PaleVioletRed1' );
```

### Text Decoration

If any of the following words appear in the method, their decoration will be added. Note, not all of these work on all ANSI consoles. Blink, for instance, doesn't seem to work in Windows.

* bold
* underscored
* blinking
* reversed - Inverse of the default terminal colors
* concealed
* indented

```javascript
print.boldText( "Don't make me turn this car around!" );
print.underscoredLine( "Have I made my point?" );
```

`indented` isn't part of the ANSI standard but rather a nice way to indent each line of output with two spaces to help clean up nested lines of output. If the string being passed in has carriage returns, each of them will be preceded by two spaces.

```javascript
print.boldText( "Header" )
    .indentedLine( "Detail 1" )
    .indentedLine( "Detail 2" )
    .indentedLine( "Detail 3" );
```

### Mix It Up

Any combination of the above is possible. Filler words like "text" will simply be ignored so you can make your method nice and readable. Get creative, but just don't overdo it. No one wants their console to look like a rainbow puked on it.

```javascript
print.redOnWhiteLine( 'Ready the cannons!' );
print.boldRedOnBlueText( "Test dirt, don't wash." );
print.boldBlinkingUnderscoredBlueTextOnRedBackground( "That's just cruel" );
```

### Dynamic Formatting

Some times you want to apply formatting at run time. For instance, show a status green if it's good and red if it's bad. You can pass a second string to the print helper with additional formatting that will be appended to the method name.

```javascript
print.text( status, ( status == 'running' ? 'green' : 'red' ) );
```

Depending on the value of the `status` variable, that line would be the same as one of the following two lines:

```javascript
print.greenText( status );
print.redText( status );
```

### Flush

If you have a task that takes a while to complete and you want to update the user right away, you can flush out everything in the buffer to the console with the .toConsole\(\) method. Note, any text flushed to the console cannot be piped to another command.

```javascript
print.Line( 'Step 1 complete' ).toConsole();
print.Line( 'Step 2 complete' ).toConsole();
print.Line( 'Step 3 complete' ).toConsole();
```

### Chaining

All the methods in the `print` object can be chained together to clean up your code.

```javascript
print.whiteOnRedLine( 'ERROR' )
    .line()
    .redLine( message )
    .line();
```

## Strip ANSI Formatting

If you have a string that contains ANSI formatting and you want to strip it out to just plain text, there is a function on the print helper to do this for you.

```javascript
var strippedOutput = print.unANSI( formattedOutput );
```



