# Piping Into Commands

The CommandBox interactive shell already allows for a command to pipe data into another Command.

```bash
cat myfile.txt | grep "find me"
```

Since CommandBox commands don't have a "standard input", the output of the previous command is passed into the second command as its first parameter.  In this instance, the `grep` command's first parameter is called `input` so it receives the value returned by the `cat` command.  The `"find me"` text becomes the second parameter-- in this case, `expression`.  

## Examples

There is nothing special about a parameter that can received piped input.  In fact, any command can receive piped into for its first parameter.  The following commands all accomplish the same thing.

```bash
coldbox create app "My App"
```

```bash
echo "My App" | coldbox create app
```

```bash
echo "My App" > appName.txt
cat appName.txt | coldbox create app
```

This allows you to get creative by combining commands together like so:

```bash
package set name="hello world"
package show name | sed s/hello/goodbye/ | package set name=''
```

This takes a package name, replaces some text, and sets it back in all in one line.  One benefit is that Windows users don't have a native `sed` command in their OS, but those commands inside a CommandBox Recipe will execute consitently on any machine.  

## Piping From The OS

What if you aren't using the interactive shell and you want to pipe into CommandBox from your OS's native shell?  This is also supported, and the piped input will become the first parameter as before.

```bash
C:\> echo coldbox | box install
C:\> echo reverse('this is a test') | box repl
```