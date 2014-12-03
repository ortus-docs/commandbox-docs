# Usage


Since CommandBox is a command line tool, it requires no GUI. If you are
using an OS with a GUI (like Windows) and you run the `box` executable,
the shell will open in a new window. CommandBox looks and behaves like a
Bash or DOS window, but the command prompt is not your native OS-- it's
CommandBox running *INSIDE* your native shell and interpreting what you
type.

![commandbox](../images/welcome.png)

## Native Shell Integration

As such, operating system commands won't execute (unless we've
implemented a similar one in CommandBox). However, we also have a command
called `run` ([See API Docs](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/run.html)) which can be used to run native commands from within
CommandBox. Command input parameters and flags won't necessarily match
your OS's native shell. CommandBox uses its own parser and
commands/parameters will be formatted as documented here.

Selecting and copy/paste may differ based on your operating system and
how it treats command windows. CommandBox uses some color and formatting
in its output. This uses [ANSI](http://en.wikipedia.org/wiki/ANSI_escape_code) escape codes and will only work correctly
if you're using an ANSI-compatible shell to run CommandBox from.

## Starting and Stopping

If you have launched the CommandBox executable from your OS's GUI, it
will run inside of a native shell window. When finished running
commands, you can simply close the window, or type "[exit][]" an the
window will close for you. If you have a series of commands to run, this
is the recommended approach.

If you manually open your native shell and execute the box executable,
it will take over the screen and the prompt will now become a CommandBox
prompt. When you type "[exit][]" the CommandBox program will drop you
back at your native OS shell.

  []: commandbox/welcome.png "commandbox/welcome.png"
  [run]: http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/run.html
  [exit]: http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/quit.html