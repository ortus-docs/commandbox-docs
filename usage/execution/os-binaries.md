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
#listlast `!pwd` /
```

## Parsing Rules

When passing a command string for native execution, ALL REMAINING TEXT in the line will be "eaten" by the native execution and passed to the OS for processing.  This is so the CommandBox parser doesn't "'screw up" any special syntax that your OS command processor is expecting.  That means any use of piping or `&&` will get passed straight to the OS.  On Windows, the following string will run the `ver` command twice in Windows.

```bash
!ver && ver
```

In the event you want to pipe the result of an OS binary to another CommandBox command or chain another CommandBox command on the end, you can workaround this by echoing out the string and then piping that to the `run` command.  This example will run the Windows `ver` command followed by the CommandBox `ver` command.  

```bash
echo "ver" | run && ver
```

Additionally, any [expansions you put in your command string with backticks](../parameters/expressions.md) or [System Setting placeholders](../system-settings.md#using-system-settings-from-the-cli) will not be processed by CommandBox, but will be passed to the native OS directly.  This Windows example won't do what you might think since the backticks are passed, untouched to the OS \(so the OS can expand them if it needs\):

```bash
!git status | find "`package show name`"
```

Instead, you can pass the command text through `echo` to have CommandBox process the backtick expansions first before sending it off to the OS for processing.

```bash
echo 'git status | find "`package show name`"' | run
```

In the above example, written for Windows, the output of the `echo` command has the `package show name` expression expanded into the string and then the ENTIRE string is piped to `run` where the pipe and the `find` command are processed by Windows. Note, there is no need for preceding the command with `!` when passing to `run` since `!` is just an alias for `run`.

When you prepare the native binary ahead of time and then pipe it into the `run` command, you are allowed to pipe the result back into another CommandBox command in that specific case.  This is only possible when `run` appears with nothing after it.

```bash
echo 'git status | find "`package show name`"' | run | #ucase
```

## Piping to the native binary's standard input

You can pipe the output of a previous command in CommandBox directly to a native binary like so: 

```bash
#createguid | !clip
or
#createguid | run clip
```

In this case, `clip` is a Windows binary that will read the standard input and place that text on the clipboard.  When the `run` command receives two inputs, it will assume the first input is the piped input and the second input is the actual command to run.  

You can even pipe commands to an interpreter that normally reads from a keyboard on the standard input, but be aware that some binaries such as Windows `cmd` require line breaks after the input or it won't process.  In the specific case of Windows `cmd` it seems to require at least two line breaks for some reason \(this is also true outside of CommandBox\)

```bash
echo "ping google.com`#chr 10``#chr 10`" | !cmd
```

In the previous example we use a backtick expansion to grab a line feed from the CFML `chr()` function.

### Limitations of piping

There are limitations.  When you pipe into the `run` command, the command will not also be able to read from your keyboard \(this is true of any shell\) and it will execute in a non-interactive manner, which means the ping's output above would appear all at once as opposed to flowing in one line at a time.

When piping into the `run` command you cannot also pipe the output of the run command like so:

```bash
#createguid | !clip | #ucase
```

As soon as any text appears after `run` or `!`, then the rest of the line is "eaten" and passed to the native shell.

Also you cannot build up a command like so and also pipe input into the native binary at the same time:

```bash
echo "clip" | run
```

This is because only one parameter can be piped into a command at a time.

## Debugging

If you're having issues getting a native binary to run, you can turn on a config setting that will echo out the exact native command being run including the call to your OS's command interpreter.  

```bash
config set debugNativeExecution=true
```

## Setting the Native Shell

You can override the default native shell from `/bin/bash` to any shell of your choosing, like zsh. This will let you use shell specific aliases. You can set your native shell property using the `config set` command \(i.e., `config set nativeShell=/bin/zsh`\)

## Exit Codes

If the native binary errors, the exit code returned will become the exit code of the `run` command itself and will be available via the usual mechanisms such as `${exitCode}`.

## CLI Environment Variables

Any environment variables you set in the CommandBox shell will be available to the native process that your OS binary runs in. Here's a Windows and \*nix example of setting an env var in CommandBox and then using it from the native shell.

```bash
set name=brad
!echo %name%
```

```bash
set name=brad
!echo $name
```





