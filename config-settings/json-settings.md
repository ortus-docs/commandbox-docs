# JSON Settings

You can customize the way that JSON is formatted when it's written to files such as `server.json` and `box.json` as well as how it displays in the console when using commands such as `server show` and `package show`

## JSON.indent

**string**

String to use for indenting lines. Defaults to four spaces.

```bash
config set JSON.indent="  "
```

## JSON.lineEnding

**string**

String to use for line endings. Defaults to CRLF on Windows and LF on \*nix.  Pass the actual character to use, not a placeholder. &#x20;

```bash
config set JSON.lineEnding=`#chr 10`
```

## JSON.spaceAfterColon

**boolean**

Add space after each colon like `"value": true` instead of `"value":true`  Defaults to false

```bash
config set JSON.spaceAfterColon=true
```

## JSON.sortKeys

**string**

Specify a sort type to sort the keys of json objects: `text` or `textnocase`

```bash
config set JSON.sortKeys=textnocase
```

## JSON.ANSIColors

**struct**

A struct of colors to use when displaying JSON in the CLI. You can use any color name from the `system-colors` command or a direct ANSI escape sequence.

## JSON.ANSIColors.constant

**string**

The color to use for constant values (true/false/null). Defaults to "red".

```bash
config set JSON.ANSIColors.constant=PaleTurquoise1
```

## JSON.ANSIColors.key

**string**

The color to use for object key names. Defaults to "blue".

```bash
config set JSON.ANSIColors.key=Purple5
```

## JSON.ANSIColors.number

**string**

The color to use for numbers. Defaults to "aqua".

```bash
config set JSON.ANSIColors.number=SeaGreen3
```

## JSON.ANSIColors.string

**string**

The color to use for quoted string values. Defaults to "lime".

```bash
config set JSON.ANSIColors.string=MistyRose3
```



