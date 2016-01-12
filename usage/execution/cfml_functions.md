# Executing CFML Functions

You can already execute CFML functions in the `REPL` command to play in a sandbox, but sometimes you want to go further and actually use CFML directly in the CLI.  This is where the `cfml` command comes in handy.  The following runs the now() function. It is the equivalent to `repl now()`.
```bash
cfml now
```

## #function

As a handy shortcut, you can invoke the `cfml` command by simply typing the name of the CFML function, preceded by a `#` sign.

```bash
#now
```

## Function parameters

When you pass parameters into this command, they will be passed directly along to the CFML function.  The following commands are the equivalent of `hash( 'mypass' )` and `reverse( 'abc' )`.

```bash
#hash mypass
#reverse abc
```

### Piping them together

This really gets useful when you start piping input into CFML functions. Like other CFML commands, piped 
input will get passed as the first parameter to the function. This allows you to chain CFML functions 
from the command line like so. (Outputs "OOF")

```bash
#listGetAt www.foo.com 2 . | #ucase | #reverse
```

By piping commands together, you can use CFML functions to transform output and input on the fly to generate very powerful one-liners that draw on the many CFML functions already out there that operate on simple values.

## Complex Values

Since this command defers to the REPL for execution, complex return values such as arrays or structs will be 
serialized as JSON on output. As a convenience, if the first input to an array or struct function looks like 
JSON, it will be passed directly as a literal instead of a string. 

The first example averages an array. The second outputs an array of dependency names in your app by manipulating the JSON object that comes back from the `package list` command.

```bash
#arrayAvg [1,2,3]
package list --JSON | #structFind dependencies | #structKeyArray
```

## Named Parameters

You must use positional parameters if you are piping data to a CFML function, but you do have the option to use named parameters otherwise. Those names will be passed along directly to the CFML function, so use the CF docs to make sure you're using the correct parameter name.

```bash
#directoryList path=D:\\ listInfo=name
```
