#Command History

Another trick the shell has up its sleeve is tracking the history of commands you type. To access recent commands, use the **up** and **down** arrow at the prompt and CommandBox will cycle through your command history.

# REPL History
The REPL command has its own history for CFML statements you run. There is a separate history for script REPL and tag REPL.

# Managing History
The history is stored in a text file in the root of your user/.CommandBox/ folder. To manage it programatically, use the [history](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/history.html) command. Here is an example of viewing and clearing your command history.

```
#View all history
history

## View all the times you played snake, paginated (because there is so many)
history | grep snake | more
history --clear
```

Read more about the [history command](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/history.html) in the Command API docs or the built-in help.

```
history help
```