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