# Executing OS Binaries

If you want to execute a native binary from inside the interactive shell or as part of a CommandBox recipe, we allow this via the `run` command.    You can read the API docs for `run` [here](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/commands/run.html).

http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/commands/run.html

> **Hint** This behavior is dependent on your operating system.

## run binary

Execute an operation system level command using the native shell. For Windows users, `cmd.exe` is used.  For Unix, `/bin/bash` is used.  Command will wait for the OS command to finish.


The binary must be in the PATH, or you can specify the full path to it.  This cannot be used for any commands that require interactivity or don't exit automatically or the call will hang indefinitely.

```bash
run myApp.exe
run /path/to/myApp
```

## !binary

A shortcut for running OS binaries is to prefix the binary with `!`.  In this mode, any other params need to be positional.  There is no CommandBox parsing applied to the command's arguments.  They are passed straight to the native shell.

```bash
!myApp.exe
!/path/to/myApp
!dir
!netstat -pan
!npm ll
!ipconfig
!ping google.com -c 4
```

Executing Java would look like this

```bash
!java -version
!java -jar myLib.jar
```

You can even call other CLIs

```bash
!git init
touch index.cfm
!git add .
!git commit -m "Initial Commit"
```

## Building On

The output of native calls can be used in [expressions](/usage/parameters/expressions.md) or piped into other commands.  Here's a Unix example that uses CFML functions from the command line to parse the parent folder from the current working directory:

```bash
!pwd | #ListLast /
```