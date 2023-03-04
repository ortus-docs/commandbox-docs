# Task Interactivity

Some tasks simply do some processing, output their results, and finish executing. You may want to interact with the user or create a long-running that can be controlled by user input (See the `snake` game)

Use these methods made available to interact with your users

## ask()

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

```
response = ask( message='What is your password?', mask='*' );
```

You can also put default text in the buffer for a wizard-style interface where the user can simply hit "enter" to accept the visible default values.

```
response = ask( message='Enter installation Directory: ', defaultResponse='/etc/foo/bar` );
```

## waitForKey()

If you just need a single character collected from a user, or perhaps any keystroke at all, use the `waitForKey()` method. A message must be supplied that lets the user know what you need. This method  can capture a single standard character and also has some special (multi-char) return values that represent special key presses. &#x20;

```javascript
var keyPress = waitForKey( 'Press any key, any key.' );
print
  .line()
  .line( 'My magic tells me you pressed: #keyPress#' );
```

If the return is not a single character, it will be one of the following special strings:

* `key_left` - Left arrow
* `key_right` -  &#x20;Right arrow
* `key_up` - Up arrow
* `key_down`  &#x20;\- Down arrow
* `back_tab` - Shift-tab.  (Regular tab will come through as a normal tab char)
* `key_home`  &#x20;\- Home key
* `key_end` - End end
* `key_dc` - Delete
* `key_ic` - Insert key
* `key_npage` -  &#x20;Page down
* `key_ppage`  &#x20;\- Page up
* `key_f1` - F1 key
* `key_f2` - F2 key
* `key_f3` - F3 key
* `key_f4` - F4 key
* `key_f5` - F5 key
* `key_f6` - F6 key
* `key_f7` - F7 key
* `key_f8` - F8 key
* `key_f9` - F9 key
* `key_f10` -  &#x20;F10 key
* `key_f11` - F11 key
* `key_f12`  &#x20;\- F12 key
* `esc` - Escape key

## confirm()

If you want to ask the user a yes or no question, use the `confirm()` method. Any boolean that evaluates to true or a `y` will return true. Everything else will return false. This allows your users to respond with what's natural to them like `yes`, `y`, `no`, `n`, `true`, or `false`. You must pass a question into the method and you will receive a boolean back.

```javascript
if( confirm( 'Do you like Pizza? [y/n]' ) ) {
    print.greenLine( 'Good for you.' );
} else {
    print.boldRedLine( 'Heretic!!' );
}
```

## Multiselect input

Sometimes you want to collect input from the user that is constrained to a limited number of predefined options.  You could have them enter via `ask()` as freetext, but  that is more prone to errors.  This is where the Multiselect input control comes in handy.   It blocks just like the ask command until the user responds but allows the user to interact with it via their keyboard.  Think of it like radio buttons or checkboxes.  If you configure it to only allow a single response (radio buttons) then a string will come back containing the answer.  If you configure it to allow multiple selections, you will receive an array of responses back, even if there was only one selection made.&#x20;

Here is a simple example that uses a comma-delimited list to define the options.

```javascript
var color =  multiselect( 'What is your favorite color? ' )
    .options( 'Red,Green,Blue' )
    .ask();
```

Here is another example that defines the options in an array.  This allows you to have different text on screen from what gets returned in the response.  This sets multiple responses on so an array will come back.  This also sets the input as required so the user will be required to select at least one option.

```javascript
var colorArray =  multiselect( 'What is your favorite color? ' )
    .options( [
        { display='Red', value='r', selected=true },
        { display='Green', value='g' },
        { display='Blue', value='b' }
    ] )
    .multiple()
    .required()
    .ask();
```

Notice how the "red" option is set as selected by default.  Even though the colors will show up as "Red", "Green", and "Blue, the values will come back in the array as "r", "g" and "b" in the array. &#x20;

Display and value are both both required in the array of options above.  If you provide at least one of the two, the other will default to the same.  A keyboard shortcut will be created for each option which defaults to the first character of the display.  So for instance, pressing "R" on your keyboard will select the Red option. Pressing "G" will select Green, etc.  You can override the shortcut with an `accessKey` setting in the struct. &#x20;

```javascript
var response = multiselect( 'What place would you like? ' )
	.options( [
		{ value='First', accessKey=1 },
		{ value='Second', accessKey=2 },
		{ value='Third', accessKey=3 }
	] )
	.ask();
```

## &#x20;Force

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
