# Commands

CommandBox uses its own command parser that should be similar to what you're used to in other shells. Commands are not case-sensitive and don't contain any special characters except an occasional dash (`-`). Each command and its parameters will be entered on a single line. Press enter when you are done typing to execute that command. If you ever need to include a line break in a parameter value, quote the value and use the `\n` escape sequence.

# Namespaces

To help organize our commands, we introduced the concept of **namespaces**. this means that commands can contain spaces and be comprised of more than one word. This is to keep things readable. Several commands that are all related will start with the same word, or namespace. An example of this is the [artifacts](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/artifacts/package-summary.html) namespace. It contains several commands inside of it including [list](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/artifacts/list.html), [clean](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/artifacts/clean.html), and [remove](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/artifacts/remove.html). Calling each of them would look like this:

```bash
artifacts list
artifacts clean
artifacts remove
```

>**Hint** : the text `artifacts` itself is not a command and you will receive an error if you hit enter after just typing that text. Context-specific help is available for all namespaces by typing `help` or `-help` after the namespace.

```bash
artifacts help
```

Namespaces can be more than one level. Another example would be [coldbox create](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/coldbox/create/package-summary.htmll) which contains commands such as [app](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/coldbox/create/app.html), [view](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/coldbox/create/view.html), and [handler](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/coldbox/create/controller.html).

```bash
coldbox create handler
```

# Aliases
Commands can be aliased so you can call them more than one way. Check the Command API docs or the CLI help command to see if a command has aliases. For instance, the quit command is aliases as q for quick typing. Another example would be the package init command that is aliased to just init.