# Tab Completion

One of the most productive features of the shell is tab completion. This means you can type a partial command and hit the tab key on your keyboard to be prompted with suggestions that match what you've typed so far. If there is only one match, it will be finished for you. This can save a lot of typing and will be a familiar concept to those already living in a CLI environment.

When "tab" is pressed, the text you've entered so far is run through the CommandBox command parser to see if it can match a namespace, command, or parameters. If you press tab at an empty prompt, all top level commands and namespaces will display. Since tab completion is run through the standard command parser, that means it works on command aliases as well.