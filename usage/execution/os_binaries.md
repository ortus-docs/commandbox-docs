# Executing OS Binaries

If you want to execute a native binary from inside the interactive shell or as part of a CommandBox recipe, we allow this via the `run` command.    You can read the API docs for `run` [here](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/commands/run.html).

http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/commands/run.html

> **Hint** This behavior is dependent on your operating system.

## run

Execute an operation system level command. By default, "run" will wait 60 seconds for the command to complete.

```bash
run myApp.exe
```

Executing Java would look like this (Assuming java is installed and in your OS's system path).
```bash
run java "-jar myLib.jar"
```

On Windows, most things you could run via a Command Prompt need to be executed through the `cmd` binary with the `/c` switch so it exits after completing.  

```bash
run cmd "/c dir"
```

## !binary

A shortcut for running OS binaries is to prefix the binary with `!`. In this mode, any other params need to be positional.

```bash
!myApp.exe
!git pull
!cmd "/c dir"
```