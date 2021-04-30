# Interactive Shell Features

When you run `box` without any parameters, you get the CommandBox interactive shell. We use a library called JLine for this interaction and it has a number of bash-like behaviors to make you more productive. CommandBox also bundles several bash-like commands to give you a consistent shell regardless of whether you're on Windows or Linux.

## Ctrl-C & Ctrl-D

Pressing Ctrl-C will send an interrupt signal to the terminal which will end any currently executing command and exit you back to the shell's prompt. Pressing Ctrl-C if you're already at the prompt won't do anything at all.

Pressing Ctrl-D from a prompt sends an OEF signal and will exit out of the shell entirely, just like if you had run the `exit` command.

## History Search

CommandBox allows you to re-run items from your command and/or REPL history by pressing the up arrow  to cycle through previous commands.  You  can type a partial command  like `cd` and then hit up arrow and the history items will be filtered to items starting with that.  You can also use what's commonly known as `i-search`.  Press Ctrl-Shift-R to open a search from the console where you can search your entire command history by keyword.  Keep pressing Ctrl-Shift-R to cycle through the results.  Press Ctrl-Shift-S to cycle backwards through the results.

## Change to Previous Directory

Like Powershell or Bash, you can type the following to switch back to the previous working directory:

```bash
cd -
```

If you run this same command more than once, you will keep toggling between the same two directories \(bash behavior\)





