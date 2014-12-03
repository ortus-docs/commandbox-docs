# Parameters

Many commands accept parameters to control how they function. Parameters are entered on the same line as the command and separated by a space and can be provided as named OR positional, similar to how CFML functions can be called. You cannot mix named and positional parameters in the same command or an error will be thrown. There is also a concept of "flag" for boolean parameters that can be combined with named or positional parameters for brevity and readability.

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



