# Commands

## Commands

CommandBox uses its own command parser that should be similar to what you're used to in other shells. Commands are not case-sensitive and don't contain any special characters except an occasional dash \(`-`\). Each command and its parameters will be entered on a single line. Press enter when you are done typing to execute that command. If you ever need to include a line break in a parameter value, quote the value and use the `\n` escape sequence.

For a full list of all the commands that ship with CommandBox as well as all their parameters and samples, please visit our [Command API docs](http://apidocs.ortussolutions.com/commandbox/current) which are auto-generated each build.

## Namespaces

To help organize our commands, we introduced the concept of **namespaces**. this means that commands can contain spaces and be comprised of more than one word. This is to keep things readable. Several commands that are all related will start with the same word, or namespace. An example of this is the [artifacts](http://apidocs.ortussolutions.com/commandbox/3.9.1/index.html?commandbox/system/modules_app/artifacts-commands/commands/artifacts/package-summary.html) namespace. It contains several commands inside of it including [list](http://apidocs.ortussolutions.com/commandbox/3.9.1/index.html?commandbox/system/modules_app/artifacts-commands/commands/artifacts/list.html), [clean](http://apidocs.ortussolutions.com/commandbox/3.9.1/index.html?commandbox/system/modules_app/artifacts-commands/commands/artifacts/clean.html), and [remove](http://apidocs.ortussolutions.com/commandbox/3.9.1/index.html?commandbox/system/modules_app/artifacts-commands/commands/artifacts/remove.html). Calling each of them would look like this:

```bash
artifacts list
artifacts clean
artifacts remove
```

> **Hint** the text `artifacts` itself is not a command and you will receive an error if you hit enter after just typing that text. Context-specific help is available for all namespaces by typing `help` after the namespace.

```bash
artifacts help
```

Namespaces can be more than one level. Another example would be [coldbox create](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules_app/coldbox-commands/commands/coldbox/create/package-summary.html) which contains commands such as [app](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules_app/coldbox-commands/commands/coldbox/create/app.html), [view](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules_app/coldbox-commands/commands/coldbox/create/view.html), and [handler](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules_app/coldbox-commands/commands/coldbox/create/handler.html).

```bash
coldbox create handler
```

## Aliases

Commands can be aliased so you can call them more than one way, ever wanted to run an `ls` command in Windows or a `dir` command in `Unix`? . Check the [Command API docs](http://apidocs.ortussolutions.com/commandbox/current) or the CLI help command to see if a command has aliases. For instance, the [quit](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/commands/quit.html) command is aliases as `q` for quick typing. Another example would be the [package init](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/package-commands/commands/package/init.html) command that is aliased to just `init`.

