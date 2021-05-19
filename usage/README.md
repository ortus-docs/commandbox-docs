# Usage

Since CommandBox is a command line tool, it requires no GUI. If you are using an OS with a GUI \(like Windows\) and you run the `box` executable, the CommandBox interactive shell will open in a new window. CommandBox looks and behaves like a Bash or DOS window, but the command prompt is not your native OS-- it's CommandBox running _INSIDE_ your native shell and interpreting what you type.

![CommandBox CLI](../.gitbook/assets/image%20%288%29%20%281%29%20%281%29.png)

## Interactive Shell

As such, operating system commands won't execute \(unless we've implemented a similar one in CommandBox\). However, we also have a command called `run` \([See API Docs](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/commands/run.html)\) which can be used to run native commands from within CommandBox. Command input parameters and flags won't necessarily match your OS's native shell. CommandBox uses its own parser and commands/parameters will be formatted as documented here.

Selecting and copy/paste may differ based on your operating system and how it treats command windows. CommandBox uses some color and formatting in its output. This uses [ANSI](http://en.wikipedia.org/wiki/ANSI_escape_code) escape codes and will only work correctly if you're using an ANSI-compatible shell to run CommandBox from.

## Starting and Stopping

If you have launched the CommandBox executable from your OS's GUI, it will run inside of a native shell window. When finished running commands, you can simply close the window, or type `exit` an the window will close for you. If you have a series of commands to run, this is the recommended approach.

If you manually open your native shell and execute the box executable, it will take over the screen and the prompt will now become a CommandBox prompt. When you type `exit` the CommandBox program will drop you back at your native OS shell.

## One-Off Commands

You can also leverage CommandBox as another operating system binary if you want to execute one-off commands without having the need to go into the interactive shell. This is useful if you are integrating CommandBox with other system binaries, task runners, ANT, or you just want to execute a CFML template or CommandBox recipe.

```bash
$ box version
$ box install coldbox-be
$ box upgrade
```

> **Info** Executing one-off commands might have a delay when executing as the CommandBox environment needs to load first.

So let's go a little deeper into [CommandBox execution.](execution/)

## Special Path Expansions

On Windows, `/` or `\` will be treated as the current drive root based on the current working directory. This is the same as DOS.

```text
cd /windows/system32
```

On all OS's, `~` will expand to the current user's home directory.

```text
cd ~
ls ~/.ssh
```

