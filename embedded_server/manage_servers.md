# Managing CommandBox Servers

CommandBox stores information about each of the servers you've ever started inside `~/.CommandBox/servers.json` so it can remember settings from one run to the next.  

## List your server

You can see an overview of your servers and what state they're in with the `server list` command.  

```bash
server list

site1 (starting)
  https://127.0.0.1:8081
  C:\site1

site1 (running)
  https://127.0.0.1:8082
  C:\site2

site3 (stopped)
  https://127.0.0.1:8083
  C:\site3
```

If you have many servers, you can provide parmeters to help filter the results from `server list`

```bash
# All servers containing the word "foo"
server list foo

# Running servers
server list --running
```

You can take a quick look at the what's been happening with the `server log` command or use the `server status` command to see more detailed information including the arguments used previously to start/stop the server. 

## Multiple Servers
Servers are uniquely identified by their full path, but they also have a short name which defaults to the immediate folder containing their web root.  The `stop`, `start`, etc commands can be run in the web root for a server, or in any working directory as long as you reference the server's short name.

```bash
start site1
start site2
restart site3
stop site2
stop --all
```

Another handy shortcut is the `server cd` command that will change the current working directory of the interactive shell to the web root of a named server.

```bash
server cd site1
start
server cd site2
install myPackage
restart
```

>**Info** Server name is the first parameter to all server commands and tab completion works too, making it as easy as possible for you.


## Forgetting Servers
If you want to wipe all configuration, logs, and WEB-INF files for a server, use the `server forget` command.  This will also remove any administrator settings you may have saved including data sources, mail servers, and server mappings.

```bash
server forget
```

You can forget all your servers at once too if you want to start with a clean slate.  This command will stop and forget all servers.

```bash
server stop --all --forget
```



