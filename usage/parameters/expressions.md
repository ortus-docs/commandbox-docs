# Expressions

Parameter values passed into a CommandBox command don't have to be static. Any part of the parameter which is enclosed in backticks (\`) will be evaluated as a CommandBox expression. That means that the enclosed text will be first executed as though it were a separate command and the output will be substituted in its place.

Any valid command can be used as an expression, including calls to native OS binaries, CFML functions, or REPL one-liners. Note that any text that a command immediately flushes to the console during its execution (like a download progress bar) will not be returned by the expression, though it will display on the console.

## Entire Parameter

Take for instance, this simple command that prints out the contents of a file:

```bash
cat defaultServer.txt
```

It can be used as a dynamic parameter like so:

```bash
server start name=`cat defaultServer.txt`
```

In the example above, the contents of the `defaultServer.txt` file will be passed in as the value of the "name" parameter to the "server start" command. If the contents of the file was the text `myServer`, the equivalent final command would be:

```bash
server start name=myServer
```

## Inside Parameters

There can be more than one expression in a single parameter value. Expressions can also be combined with static text and they will all be evaluated in the order they appear:

```bash
echo "Your CommandBox version is `ver` and this app is called '`package show name`'!!"
```

That would output something similar to:

```
Your CommandBox version is 3.0.0 and this app is called 'Brad's cool app'!
```

If you need to use an actual backtick in a parameter value, escape it with a backslash.

```bash
echo "Nothing to \`see\` here"
```

Which outputs

```
Nothing to `see` here
```

## Express Yourself

This unlocks a new world of scripting potential when combined with other abilities like native OS binary execution and CFML functions from the CLI. Here's some examples to get your gears turning:

Set a package property in `box.json` equal to the current date passed through a CFML date mask

```bash
package set createdDate="'`#now | #dateformat mm/dd/yyyy`'"
Set createdDate = 1/1/2016
```

Set properties based on manipulations of previous values:

```bash
package set name=brad
Set name = brad
package set name="`package show name` wood"
Set name = brad wood
```

Perform CFML operations on local files:

```bash
Commandbox> #hash `cat pass.txt`
```

Execute environment-aware install scripts based on local files. (`isProduction.txt` would contain the text `true` or `false` in this ex.)

```bash
install id=coldbox production=`cat /home/user/isProduction.txt`
```
