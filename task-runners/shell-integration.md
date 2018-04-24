# Shell Integration

Your task might need to get information about its environment or perhaps proxy to other commands. Here is a handful of useful methods available to all commands.

## getCWD\(\)

This method will return the Current Working Directory that the user has changed to via the `cd` command. The path will be expanded and fully qualified.

## shell.clearScreen\(\)

This method on the shell object will clear all text off the screen and redraw the CommandBox prompt.

## shell.getTermWidth\(\)

This shell method returns the number of characters wide that the terminal is. Can be useful for outputting long lines and making sure they won't wrap.

## shell.getTermHeight\(\)

This shell method returns the number of characters tall the terminal is. Can be useful for [outputting ASCII art](https://github.com/bdw429s/CommandBox-Image-To-ASCII).

