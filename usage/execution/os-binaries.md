# OS Binaries

If you want to execute a native binary from inside the interactive shell or as part of a CommandBox recipe, we allow this via the `run` command. You can read the API docs for `run` [here](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/run.html).

[http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/run.html](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/run.html)

> **Hint** This behavior is dependent on your operating system.

## Using `run binary`

Execute an operation system level command using the native shell. For Windows users, `cmd.exe` is used. For Unix, `/bin/bash` is used. Command will wait for the OS command to finish.

The binary must be in the PATH, or you can specify the full path to it. Your keyboard will pass through to the standard input stream of the process if it blocks for input and the standard output and error streams of the process will be bound to your terminal so you see output as soon as it is flushed by the process.

```bash
run myApp.exe
run /path/to/myApp
```

## Using `!binary`

A shortcut for running OS binaries is to prefix the binary with `!`. In this mode, any other params need to be positional. There is no CommandBox parsing applied to the command's arguments. They are passed straight to the native shell. As such, you don't need to escape any of the parameters for CommandBox when using this syntax.

```bash
!myApp.exe
!/path/to/myApp
!dir
!netstat -pan
!npm ll
!ipconfig
!ping google.com -c 4
!java -jar myLib.jar
```

## Current Working Directory Aware

OS Commands you run are executed in the same working directory as CommandBox. This means you can seamlessly invoke other CLIs without ever leaving the interactive shell.

```bash
!git init
touch index.cfm
!git add .
!git commit -m "Initial Commit"
```

## Building On

The output of native calls can be used in [expressions](../parameters/expressions.md) or piped into other commands. Here's a Unix example that uses [CFML functions](cfml-functions.md) from the command line to parse the parent folder from the current working directory:

```bash
!pwd | #ListLast /
```

## Parsing Rules

When passing a command string for native execution, any piping, redirection, `&&` or `||` will be processed on the CommandBox side. In the above example, the native `!pwd` output is passed back to a CommandBox command.

Additionally, any [expansions you put in your command string with backticks](../parameters/expressions.md) or [System Setting placeholders](../system-settings.md#using-system-settings-from-the-cli) will not be processed by CommandBox, but will be passed to the native OS directly.

In the event you want to have the piping done by the operating system OR you want expansions to be processed by CommandBox prior to passing to the OS, you can workaround this by echoing out what you want to run and then piping that to the `run` command.

```bash
echo 'git status | find "`package show name`"' | run
```

In the above example, written for Windows, the output of the `echo` command has the `package show name` expression expanded into the string and then the ENTIRE string is piped to `run` where the pipe and the `find` command are processed by Windows. Note, there is no need for preceding the command with `!` when passing to `run` since `!` is just an alias for `run`.

## Setting the Native Shell

You can override the default native shell from `/bin/bash` to any shell of your choosing, like zsh. This will let you use shell specific aliases. You can set your native shell property using the `config set` command \(i.e., `config set nativeShell=/bin/zsh`\)

## Exit Codes

If the native binary errors, the exit code returned will become the exit code of the `run` command itself and will be available via the usual mechanisms such as `${exitCode}`.

## Debugging

If you're having issues getting a native binary to run, you can turn on a config setting that will echo out the exact native command being run including the call to your OS's command interpreter.  

```bash
config set debugNativeExecution=true
```





