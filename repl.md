# REPL (Read Eval Print Loop)

CommandBox contains a `REPL` command which is a powerful tool to execute ad-hoc CFML code from the command line.  A REPL reads user input, evaluates it, prints the result, and then repeats the process.  The CommandBox REPL supports the inline execution of both CF script or tags. 

```bash
CFSCRIPT-REPL: 5+5
=> 10
```

## Script REPL
The default mode of the `REPL` command is to accept script.  You can enter most any CF Script into the prompt for execution.  If the script is an expression that returns a value, or sets a variable, that value/variable will be output.

```bash
CFSCRIPT-REPL: breakfast = ['bacon','eggs']
=> [
    "bacon",
    "eggs"
]
CFSCRIPT-REPL: breakfast.len()
=> 2
CFSCRIPT-REPL: breakfast.append( 'orange juice' )
=> [
    "bacon",
    "eggs",
    "orange juice"
]
```

### Multi-line

Multi-line statements are also allowed.  If you have typed a starting `{` without an ending `}`, the REPL will keep accepting lines until it has determined the statement to be finished.  

```bash
```