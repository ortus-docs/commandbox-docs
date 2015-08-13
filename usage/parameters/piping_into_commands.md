# Piping Into Commands

The CommandBox interactive shell already allows for a command to pipe data into another Command.

```bash
cat myfile.txt | grep "find me"
```

Since CommandBox commands don't have a "standard input", the output of the previous command is passed into the second command as its first parameter.  In this instance, the `grep` command's first parameter is called `input` so it receives the value returned by the `cat` command.  The `"find me"` text becomes the second parameter-- in this case, `expression`.  

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