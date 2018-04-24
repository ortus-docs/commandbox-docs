# Aliases

A command can go by more than one name. For instance, the `dir` command can also be called as `ls`, `ll`, or `directory`. Set a comma-delimited list of aliases in your component declaration like so:

```javascript
component aliases="ls,ll,directory" {
}
```

Command aliases don't even have to be in the same namespace. For instance, the `server start` command has an alias of just `start`. This can be very powerful as you can install a command that "overwrites" a built-in command by aliasing itself as that command.

With power comes responsibility though. Make sure you don't distribute a command that accidentally stomps all over another command. Unless your command name is decidedly unique, it's probably best for most custom commands to use a namespace to avoid conflicts.

