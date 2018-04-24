# Interactive Shell Features

When you run \`box\` without any parameters, you get the CommandBox interactive shell.

## 

Ctrl-C

Each OS tends to handle things a bit differently, but as of CommandBox 3.4.0, there is better support for capturing Ctrl-C from the user, especially on Windows.

* If a command is waiting on user input, the command will exit and go back to the shell
* If CommandBox is sitting at the interactive shell, the process will quit 



