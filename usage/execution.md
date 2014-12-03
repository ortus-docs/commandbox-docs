# Execution

There are two ways to run commands via CommandBox: inside the CommandBox interactive
shell, or one-at-a-time commands from your native shell.

## Multiple Commands

If you open the interactive shell, you will see the CommandBox splash screen (ASCII
art) an d then you'll be presented with the `commandbox\>` prompt.
You can enter as many commands as you wish in order and after each
command is finished executing, you will be returned to the CommandBox
prompt. If you have multiple commands you want to execute manually, this
is the fastest method since CommandBox only loads once. This is also the
only way to make use of features like [tab complete](usage/tab_completion.md) and [command
history](usage/command_history.md).

This example show running the `box.exe` executable from a Windows DOS
prompt, executing the [version](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/version.html), [pwd](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/pwd.html), and [echo](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/echo.html) commands, and
then exiting back to DOS.

```bash
C:\>box.exe

   _____                                          _ ____            
  / ____|                                        | |  _ \           
 | |     ___  _ __ ___  _ __ ___   __ _ _ __   __| | |_) | _____  __
 | |    / _ \| '_ ` _ \| '_ ` _ \ / _` | '_ \ / _` |  _ < / _ \ \/ /
 | |___| (_) | | | | | | | | | | | (_| | | | | (_| | |_) | (_) >  < 
  \_____\___/|_| |_| |_|_| |_| |_|\__,_|_| |_|\__,_|____/ \___/_/\_\  v1.0.0.00093

Welcome to CommandBox!
Type "help" for help, or "help [command]" to be more specific.
CommandBox> version
CommandBox 1.0.0.00093
CommandBox> pwd
C:\
CommandBox> echo "Hello World!"
Hello World!
CommandBox> exit

C:\>
```

## One-Off Commands

You can also spin up CommandBox from your native shell to execute a single command inline. You can do this if you only have one command to run, or you want to automate a command from a Unix shell script or Windows batch file. This mode will not show the ASCII splash screen, but keep in mind it still loads CommandBox up and unloads it in the background. Any output from the command will be left on your screen, and you will be returned to your native OS prompt.

Here is an example of running the version command from a Windows DOS screen. Note, you'll need to either do this from the directory that holds the box executable, or add the executable to your default command path so it is found.

```bash
C:\>box version
CommandBox 1.0.0.00093

C:\>
```

The `box` text is calling the CommandBox binary, and the `version` bit is passed along to the CommandBox shell to execute once it loads.

### Recipes
If you want to automate several commands from your native shell, it will
be faster to use our **[recipe](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/recipe.html)** command that allows you to run
several CommandBox commands at once. This will allow you to only load
the CommandBox engine once for all those commands, but still be dumped
back at your native prompt when done. Read more about the
**recipe** command in our [Command API docs](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/recipe.html).

## Debug Mode
You can also activate CommandBox in **debug** mode by passing the `-debug` flag in the command line.  This will give you much more verbose information about the running CommandBox environment.  This affects both the interactive shell or one-off commands

```bash
box version -debug
box server start -debug
```

## Output

Output from commands will be ANSI-formatted text which, by default,
streams directly to the console. You can capture the output of commands
and manipulate it, search it, or write it to a file. Use a pipe (`|`)
to pass the output of one command into another command as its first
input. Output can be piped between more than one command. Use a right
bracket (`>`) and double right bracket (`>>`) to redirect output
to the file system.

### Search

Pipe output into the **[grep](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/grep.html)** command to apply a regex upon it.
**[grep](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/grep.html)** will only emit lines matching the regex.

```bash
cat myLogFile.txt | grep "variable .* undefined"
```

### Pagination

Pipe output into the **[more](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/more.html)** command to output it line-by-line or
page-by-page. Press the spacebar to advance one line at a time. Press
the Enter key to advance one page at a time. Press ESC or “q” to abort
output.

```bash
forgebox show | more
```

### Redirection

Redirect output into a file, overwriting if it exits like so:

```bash
dir > fileList.txt
```

[API Docs for fileWrite.][]

Use the double arrows to append to an existing file.

``` {.javascript}
echo "Step 3 complete" >> log.txt
```

[API Docs for fileAppend.](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/fileWrite.html)
