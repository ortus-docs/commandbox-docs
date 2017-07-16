# Ad-hoc Command Aliases

Do you ever get tired of typing some built in commands and you'd like to just alias them as something simpler? You can create arbitrary alises now that reduce the amount of typing you do.

```
config set command.aliases.cca="coldbox create app"
cca myApp
```
In the above example, it's the same thing as typing `coldbox create app myApp`.

Aliases are treated as in-place shell expansions so you can alias anything including default parameters as well as multiple commands chained together.

```
config set command.aliases.foobar="echo brad | grep brad | sed s/rad/foo/ > foo.txt && cat foo.txt"
foobar
```
In the above example, typing `foobar` is the same as running the giant command string that's being set into the alias.

If you create an alias that matches an existing command name, your alias will take precedence since aliases are expanded before commands are resolved.

Here we can change the name of a common command like `echo` that we really wish had been named `print`.

```
config set command.aliases.print=echo
print brad
```

