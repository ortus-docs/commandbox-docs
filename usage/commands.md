# Commands

CommandBox uses its own command parser that should be similar to what you're used to in other shells. Commands are not case-sensitive and don't contain any special characters except an occasional dash (`-`). Each command and its parameters will be entered on a single line. Press enter when you are done typing to execute that command. If you ever need to include a line break in a parameter value, quote the value and use the `\n` escape sequence.

# Namespaces

To help organize our commands, we introduced the concept of **namespaces**. this means that commands can contain spaces and be comprised of more than one word. This is to keep things readable. Several commands that are all related will start with the same word, or namespace. An example of this is the `artifacts` namespace. It contains several commands inside of it including `list, clean, and remove`. Calling each of them would look like this:

```bash
artifacts list
artifacts clean
artifacts remove
```

>**Hint** : the text `artifacts` itself is not a command and you will receive an error if you hit enter after just typing that text. Context-specific help is available for all namespaces by typing `help` or `-help` after the namespace.