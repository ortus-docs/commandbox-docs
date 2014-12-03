# Tab Completion

One of the most productive features of the shell is tab completion. This means you can type a partial command and hit the tab key on your keyboard to be prompted with suggestions that match what you've typed so far. If there is only one match, it will be finished for you. This can save a lot of typing and will be a familiar concept to those already living in a CLI environment.

When "tab" is pressed, the text you've entered so far is run through the CommandBox command parser to see if it can match a namespace, command, or parameters. If you press tab at an empty prompt, all top level commands and namespaces will display. Since tab completion is run through the standard command parser, that means it works on command aliases as well.

## Commands
If your text matches only command, namespace, or alias, it will be auto-filled in for you. For instance, if you type the following and press tab...

```
cold
```

the [coldbox](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/coldbox/package-summary.html) namespace will be filled in and followed by a space so you are ready to continue typing.

```
coldbox 
```

If you then press tab again, you will be presented with a list of second-level namespaces inside of [coldbox](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/coldbox/package-summary.html) and the same prompt will be output again below it so you can continue typing.

```
CommandBox> coldbox
war test-watch notes create reinit module compile help
stats test-directory interceptor test report integrate
CommandBox> coldbox
```


## Parameters
If the parser finds a complete command, it will move on to parameter completion which is slightly more complicated since at first, there is no way to tell if you are going to named parameters, positional parameters, and/or flag. Based on what parameters you've typed so far, if any, CommandBox will do it's best to give you only relevant options. If it is unsure, it will provide you with every possibility it can think of. Don't be afraid to try pressing tab while typing parameters, you may be surprised how often we can guess where you're going!

Here is CommandBox giving every option possible for the [delete](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/delete.html) command. Note, force and recurse are booleans, so they can be specified as flags.

```
CommandBox> delete
path= force= --force recurse= --recurse
```

### Named
If you have started typing named parameters, CommandBox will only suggest unused named parameters and flags.

```
CommandBox> delete path=myDir force=true
 recurse= --recurse
CommandBox> delete path=myDir force=true
```

If you are using named parameters, and you have typed the name of a parameter followed by an equals sign and no space, CommandBox will attempt to prompt valid values. This includes but is not limited to booleans and file system paths.

Here, **true** and **false** are offered as possible values for the **force** parameter.

```
CommandBox> delete path=myDir force=
true false
CommandBox> delete path=myDir force=
```

Here, all files and folders in the current working directory are offered as possibilities for the **path** parameter of the [delete](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/delete.html) command.

```
CommandBox> delete path=
.project Application.cfc box.json coldbox/ config/ favicon.ico handlers/ includes/
index.cfm interceptors/ layouts/ lib/ model/ modules/ remote/ robots.txt
tests/ views/
CommandBox> delete path=
```

### Positional
Tab completion for positional parameters works the same as the "value" portion of named parameters. Parameter names will also show up when you hit tab even when using positional parameters. This is on purpose to remind you of what options you have, but you obviously won't type them.







