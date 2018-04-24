# Task Interactivity

Some tasks simply do some processing, output their results, and finish executing. You may want to interact with the user or create a long-running that can be controlled by user input \(See the `snake` game\)

Use these methods made available to interact with your users

## ask\(\)

The `ask()` task will wait for the to enter any amount of text until a line break is entered. A message must be supplied that lets the user what you'd like to receive from them. The task will return their response in a string variable.

```javascript
var favoriteColor = ask( 'WHAT, is your favorite color??' );

if( favoriteColor == 'red' ) {
    print.boldRedLine( 'AAAAHHHHHH!!!!' );
} else if ( favoriteColor == 'I mean blue' ) {
    print.line( 'Obscure Monty Python reference' );
}
```

You can mask sensitive input so it doesn't show on the screen:

```text
response = ask( message='What is your password?', mask='*' );
```

You can also put default text in the buffer for a wizard-style interface where the user can simply hit "enter" to accept the visible default values.

```text
response = ask( message='Enter installation Directory: ', defaultResponse='/etc/foo/bar` );
```

## waitForKey\(\)

If you just need a single character collected from a user, or perhaps any keystroke at all, use the `waitForKey()` method. A message must be supplied that lets the user know what you need. This method  can capture a single standard character and also has some special \(multi-char\) return values that represent special key presses.  

```javascript
var keyPress = waitForKey( 'Press any key, any key.' );
print
  .line()
  .line( 'My magic tells me you pressed: #keyPress#' );
```

If the return is not a single character, it will be one of the following special strings:

* key\_left   - Left arrow
* key\_right -   Right arrow
* key\_up   - Up arrow
* key\_down   - Down arrow
* back\_tab   - Shift-tab.  \(Regular tab will come through as a normal tab char\)
* key\_home   - Home key
* key\_end   - End end
* key\_dc   - Delete
* key\_ic   - Insert key
* key\_npage -   Page down
* key\_ppage   - Page up
* key\_f1   - F1 key
* key\_f2   - F2 key
* key\_f3   - F3 key
* key\_f4   - F4 key
* key\_f5   - F5 key
* key\_f6   - F6 key
* key\_f7   - F7 key
* key\_f8   - F8 key
* key\_f9   - F9 key
* key\_f10 -   F10 key
* key\_f11   - F11 key
* key\_f12   - F12 key
* esc   - Escape key



## confirm\(\)

If you want to ask the user a yes or no question, use the `confirm()` method. Any boolean that evaluates to true or a `y` will return true. Everything else will return false. This allows your users to respond with what's natural to them like `yes`, `y`, `no`, `n`, `true`, or `false`. You must pass a question into the method and you will receive a boolean back.

```javascript
if( confirm( 'Do you like Pizza? [y/n]' ) ) {
    print.greenLine( 'Good for you.' );
} else {
    print.boldRedLine( 'Heretic!!' );
}
```

## Force

Remember that while interactivity is cool, people might want to automate your tasks as part of a script that runs headlessly. Therefore you should always provide a way to skip prompts if possible.

```javascript
function run( required path, Boolean force=false )  {
    if( arguments.force || confirm( "Are you sure? [y/n]" ) ) {
        fileDelete( arguments.path );
        print.redLine( "It's gone, baby." );
        return;
    }
    print.redLine( "Chickened out!" );
```

