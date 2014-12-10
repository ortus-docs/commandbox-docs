# Parameters

Many commands accept parameters to control how they function. Parameters are entered on the same line as the command and separated by a space and can be provided as named OR positional, similar to how CFML functions can be called. You cannot mix named and positional parameters in the same command though or an error will be thrown. There is also a concept of "flag" for boolean parameters that can be combined with named or positional parameters for brevity and readability.

## Named Parameters
Named parameters can be specified in any order and follow the format `name=value`. Multiple named parameters are separated by a space.

```bash
coldbox create app name=myApp skeleton=AdvancedScript directory=myDir init=true
```

## Positional Parameters
Positional parameters omit the `name=` part and only use the value. They must be supplied in the order shown in the [Command API docs](http://apidocs.ortussolutions.com/commandbox/current) or help command. We try to place the most common parameters at the beginning so you can use named parameters easily.  Here is the equivalent of the named command above:

```bash
coldbox create app myApp AdvancedScript myDir true
```

Of course, only the required parameters must be specified. I'm only including all of them here for the completeness of the example.

## Required Parameters
If you do not provide a parameter that is required for the command execution, the shell will stop and ask you for each of the missing parameters before the command will execute.

```bash
CommandBox> mkdir
Enter directory (The directory to create) : myDir
Created C:\myDir
CommandBox>
```

>**Info** : It is not necessary to escape special characters in parameter values that are collected in this manner since the shell doesn't need to parse them. The exact value you enter is used.

## Flags
Any parameter that is a boolean type can be specified as a flag in the format `--name`. Flags can be mixed with named or positional parameters and can appear anywhere in the list. Putting the flag in the parameter list sets that parameter to true. This can be very handy if you want to use positional parameters on a command with a large amount of optional parameters, but you don't want to specify all the in-between ones.

```bash
coldbox create app myApp --init --installColdBox
```

You can also negate a flag by putting an exclamation point before the name in the format `--!name`. This sets the parameter to false which can be handy to turn off features that default to true.

```bash
coldbox create app myApp --!init
```

## Escaping Special Characters
If a value is a single word with no special characters, you don't need to escape anything. Certain characters are reserved as special characters though for parameters since they demarcate the beginning and end of the actual parameter and you'll need to escape them properly. These rules apply the same to named and positional parameters.

### Spaces
If a parameter has any white space in it, you'll need to wrap the value in single or double quotes. It doesn't matter which kind you use and it can vary from one parameter to another as long as they match properly.

```bash
echo 'Hello World'
echo "Good Morning Vietnam"
```

### Quotes
Quotes are actually allowed unescaped in a value like so:

```bash
echo O'reilly
```

However, if the parameter contains whitespace and is surrounded by quotes, you'll need to escape them with a backslash.

```bash
echo 'O\'reilly Auto Parts'
echo "Luis \"The Dev\" Majano"
```

>**Hint** : Only like quotes need to be escaped. Single quotes can exist inside of double and vice versa without issue. These examples below are perfectly valid.

```bash
echo "O'reilly Auto Parts"
echo 'Luis "The Dev" Majano'
```

### Equals Signs
If you have an equals sign in your value, you'll need to escape it with a backlash.

```bash
echo 2+2\=4
```

### Line Breaks
A new line can be specified with the text `\n`. Keep in mind, some parameters might not expect new lines to exist and could error.

```bash
package set description="first line\nSecond Line\nThird Line"
```

### Backslash
Since the backslash is used as our escape character you'll need to escape any legitimate backslash that happens to precede a single quote, double quote, equals sign, or letter n.


```bash
echo foo\\\=bar
```

This will print `foo\=bar`


## File Paths
Many Commands accept a path to a folder or file on your hard drive. You can specify a fully qualified path that starts at your drive root, or a relative path that starts in your current working directory. To find your current working directory, use the `pwd` command (Print Working Directory). To change your current working directory, use the `cd` command.

Here is a fully qualified path in Windows and Unix-based:

```bash
mkdir C:\sites\test
mkdir \opt\var\sites\test
```

For a relative path, do not begin with a slash.

```
mkdir test
```

File system paths will be canonicalized automatically which means the following is also valid:

```bash
mkdir ../../sites/test
```




