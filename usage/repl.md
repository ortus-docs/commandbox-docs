# REPL (Read Eval Print Loop)

CommandBox contains a `REPL` command which is a powerful tool to execute ad-hoc CFML code from the command line.  A REPL reads user input, evaluates it, prints the result, and then repeats the process.  The CommandBox REPL supports the inline execution of both CF script or tags. 

```bash
CFSCRIPT-REPL: 5+5
=> 10
```

## Directory
The default directory of the `REPL` command is the current working directory.  This allows you to easily instantiate components in your current project.  You can override this directory by passing in a `directory` option.

```bash
repl # uses current directory ( getCWD() )
# OR
repl directory="/Users/default/Code/project"
```

## Script REPL
The default mode of the `REPL` command is to accept script.  You can enter most any CF Script into the prompt for execution.  If the script is an expression that returns a value, or sets a variable, that value/variable will be output.  Variables that you set will be available to you until you exit the `REPL` command.

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

Multi-line statements are also allowed.  If you have typed a starting `{` without an ending `}`, the REPL will keep accepting lines until it has determined the statement to be finished.  The prompt changes to `...` until the statement is finished.

```bash
CFSCRIPT-REPL: for( item in breakfast ) {
...echo( item & chr(10) )
...}
=> bacon
eggs
orange juice
```

If you would like to abort a multi-line statement, simply type `exit` at the prompt.  

## Tag REPL
You can also enter tags at the REPL.  Switch to this mode by setting the `script` flag to false.

```bash
REPL --!script
```

Any output from the tags will be returned to the console.
```bash
CFML-REPL: plain text
=> plain text
CFML-REPL: <cfset name = "Brad Wood">
=>
CFML-REPL: <cfoutput>#reverse( name )#</cfoutput>
=> dooW darB
CFML-REPL: <cfif 1 eq 2>yes<cfelse>no</cfif>
=> no
```

Multi-line statements are not currently supported in the tag REPL.

## History

The script and tag REPL have their only history.  Use the `up` and `down` arrows to access previous things you typed.  Your REPL history can be viewed and managed by the `history` command (once you exit the REPL).

```bash
history type=scriptrepl
history type=tagrepl --clear
```

Tab completion is currently not supported in either of the REPLs.



