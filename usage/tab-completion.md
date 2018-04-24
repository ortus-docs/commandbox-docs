# Tab Completion

One of the most productive features of the shell is tab completion. This means you can type a partial command and hit the tab key on your keyboard to be prompted with suggestions that match what you've typed so far. If there is only one match, it will be finished for you. This can save a lot of typing and will be a familiar concept to those already living in a CLI environment.

When "tab" is pressed, the text you've entered so far is run through the CommandBox command parser to see if it can match a namespace, command, or parameters. If you press tab at an empty prompt, all top level commands and namespaces will display. Since tab completion is run through the standard command parser, that means it works on command aliases as well.

The tab completion options are broken up into groups to visualize the commands, parameters, flags, etc.  As you type, the list of available options will auto-filter for you.  You can keep hitting tab to toggle through the available options and you can press enter to select the one you want.

## Commands

If your text matches only command, namespace, or alias, it will be auto-filled in for you. For instance, if you type the following and press tab...

```text
CommandBox> cold
```

the [coldbox](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/coldbox-commands/commands/coldbox/package-summary.html) namespace will be filled in and followed by a space so you are ready to continue typing.

```text
CommandBox> coldbox
```

If you then press tab again, you will be presented with a list of second-level namespaces inside of [coldbox](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/coldbox-commands/commands/coldbox/package-summary.html) and the same prompt will be output again below it so you can continue typing.

```text
CommandBox> coldbox

Commands
  help
  reinit  (This command will reinitialize a running ColdBox application if a server was started with CommandBox)
Namespaces
  create
```

## Parameters

If the parser finds a complete command, it will move on to parameter completion which is slightly more complicated since at first, there is no way to tell if you are going to named parameters, positional parameters, and/or flag. Based on what parameters you've typed so far, if any, CommandBox will do it's best to give you only relevant options. If it is unsure, it will provide you with every possibility it can think of. Don't be afraid to try pressing tab while typing parameters, you may be surprised how often we can guess where you're going!

Here is CommandBox giving every option possible for the [delete](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/delete.html) command. Note, force and recurse are booleans, so they can be specified as flags.

```text
CommandBox> delete

Flags
  --force     (Force deletion without asking)    --recurse   (Delete sub directories)
Parameters
  path=        (file or directory to delete.)    recurse=    (Delete sub directories)
  force=      (Force deletion without asking)
```

### Named

If you have started typing named parameters, CommandBox will only suggest unused named parameters and flags.

```text
CommandBox> delete path=myDir force=true

Flags
  --recurse  (Delete sub directories)
Parameters
  recurse=   (Delete sub directories)
```

If you are using named parameters, and you have typed the name of a parameter followed by an equals sign and no space, CommandBox will attempt to prompt valid values. This includes but is not limited to booleans and file system paths.

Here, **true** and **false** are offered as possible values for the **force** parameter.

```text
CommandBox> delete path=myDir force=

Values
  force=true     force=false
```

Here, all files and folders in the current working directory are offered as possibilities for the **path** parameter of the [delete](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/delete.html) command.

```text
CommandBox> delete path=

Directories
  path=tests/       path=models/      path=coldbox/     path=modules/
Files
  path=box.json     path=index.cfm    path=server.json
```

### Positional

Tab completion for positional parameters works the same as the "value" portion of named parameters. Parameter names will also show up when you hit tab even when using positional parameters. This is on purpose to remind you of what options you have, but you obviously won't type them.

### Flags

Tab completion will always work for flags if your command has any boolean parameters. Here we type `--` in the delete command and we are prompted with `--force` and `--recurse`.

```text
CommandBox> delete myDir --

Flags
  --force  (Force deletion without asking)   --recurse   (Delete sub directories)
```

### Custom

Commands have the ability to give hints in the form of a static list or a runtime function with dynamic output.

Here the [forgebox show](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/forgebox-commands/commands/forgebox/show.html) command dynamically provides completion for its `type` attribute based on the current types returned by the [ForgeBox REST API](http://wiki.coldbox.org/wiki/ForgeBox:API-Documentation.cfm).

```text
CommandBox> forgebox show type=

Values
type=di                      type=caching                 type=projects
type=cms                     type=logging                 type=cf-engines
type=mvc                     type=modules                 type=interceptors
type=demos                   type=plugins                 type=wirebox-aspects
```

## REPL

When writing code inside the REPL, you can also press tab to get completion on 

* CFML function names
* Member function names like `.append()`
* Variable names you have created as part of your REPL session

