# Command Help

Help is integrated at every level in CommandBox.  You can help global help, namespace help, or command help at any time.  

## Global Help

To get an overall list of all the commands you have available to run, simply type `help` at the shell.

```bash
CommandBox > help
**************************************************
* CommandBox Help
**************************************************

Here is a list of commands in this namespace:
 cat
 cd
 dir
 echo
 ...

Here is a list of nested namespaces:
 coldbox
 forgebox
 package
 server
 ...

To get further help on any of the items above, type "help command name".
```
## Namespace Help
Next, drill down and get help on a specific namespace like `server`.

```bash
CommandBox >  server help
**************************************************
* CommandBox Help for server
**************************************************

Here is a list of commands in this namespace:
server forget
server log
server open
server restart
server start
server status
server stop

To get further help on any of the items above, type "help command name".
```








