# Parameters

Many commands accept parameters to control how they function. Parameters are entered on the same line as the command and separated by a space and can be provided as named OR positional, similar to how CFML functions can be called. You cannot mix named and positional parameters in the same command or an error will be thrown. There is also a concept of "flag" for boolean parameters that can be combined with named or positional parameters for brevity and readability.

## Named Parameters
Named parameters can be specified in any order and follow the format `name=value`. Multiple named parameters are separated by a space.

```bash
coldbox create app name=myApp skeleton=AdvancedScript directory=myDir init=true
```

## Positional Parameters
Positional parameters omit the `name=` part and only use the value. They must be supplied in the order shown in the Command API docs or help command. We try to place the most common parameters at the beginning so you can use named parameters easily.

Here is the equivalent of the named command above:

coldbox create app myApp AdvancedScript myDir true
Of course, only the required parameters must be specified. I'm only including all of them here for the completeness of the example.