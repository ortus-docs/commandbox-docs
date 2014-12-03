# Shell

There are two ways to run commands via CommandBox: inside the CommandBox interactive
shell, or one-at-a-time from your native shell.

## Multiple Commands

If you open the interactive shell, you will see the CommandBox splash screen (ASCII
art) an d then you'll be presented with the `commandbox\>` prompt.
You can enter as many commands as you wish in order and after each
command is finished executing, you will be returned to the CommandBox
prompt. If you have multiple commands you want to execute manually, this
is the fastest method since CommandBox only loads once. This is also the
only way to make use of features like [tab complete](usage/tab-completion.md) and [command
history](usage/command-history.md).

This example show running the box.exe executable from a Windows DOS
prompt, executing the [version][], [pwd][], and [echo][] commands, and
then exiting back to DOS.

``` {.javascript}
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

One-Off Commands
----------------

You can also spin up CommandBox from your native shell to execute a
single command inline. You can do this if you only have one command to
run, or you want to automate a command from a Unix shell script or
Windows batch file. This mode will not show the ASCII splash screen, but
keep in mind it still loads CommandBox up and unloads it in the
background. Any output from the command will be left on your screen, and
you will be returned to your native OS prompt.

Here is an example of running the version command from a Windows DOS
screen. Note, you'll need to either do this from the directory that
holds the box executable, or add the executable to your default command
path so it is found.

``` {.javascript}
C:\>box version
CommandBox 1.0.0.00093

C:\>
```

The **box** text is calling the CommandBox binary, and the
**[version][]** bit is passed along to the CommandBox she

  [tab complete]: http://www.ortussolutions.com/products/commandbox/docs/current/usage/tab-completion
  [command history]: http://www.ortussolutions.com/products/commandbox/docs/current/usage/history
  [version]: http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/version.html
  [pwd]: http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/pwd.html
  [echo]: http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/echo.html