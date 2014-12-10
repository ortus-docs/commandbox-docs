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

If the method has the word "line" in it, a new line will be added to the end of the string

'''javascript
print.line( 'I like Spam.' );
'''

### Text Color

If the method has one of the 8 ANSI colors in it, the text will be colored.

'''javascript
print.magenta( 'I sound like a toner cartridge' );
print.greenText( "You wouldn't like me when I'm angry" );
print.somethingBlue( 'UHF' );
'''
