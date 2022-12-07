# Code Quality Tools

Here's some commands to help with code quality.  You can use them as a one-time clean up and then use them as part of your regular build to maintain your coding rules.

## Normalize Indents (Tabs vs Spaces)

Ahh, the age-old debate of tabs vs spaces.  Make sure you have a solid discussion with your team and decide which one is correct (tabs, obviously!) and then use this command to implement it across your entire code base.

### From the CLI

```
utils normalize-indents **.cf?
```

### From CFML

```
command( 'indents' )
    .params( '**.cf*' )
    .run();
```

## Remove Trailing Spaces

Removes trailing whitespace from the ends of your lines.

### From the CLI

```
utils remove-trailing-spaces **.cf*
```

### From CFML

```javascript
command( 'rts' )
    .params( '**.cf*' )
    .run();
```

## Add final EOL character

Makes sure the last line of every source file has an EOL character.

### From the CLI

```
utils add-eol-at-eof **.cf*
```

### From CFML

```javascript
command( 'eol' )
    .params( '**.cf*' )
    .run();
```
