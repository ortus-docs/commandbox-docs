# Managing CommandBox Servers

CommandBox stores information about each of the servers you've ever started inside `~/.CommandBox/servers.json` so it can remember settings from one run to the next.  You can see an overview of your servers and what state they're in with the `server list` command.  

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

>**Note** If the server is killed by an outside process other than the `stop` command, CommandBox will still think it's running.  Use the `--force` flag next time you start it.

You can take a quick look at the what's been happening with the `server log` command or use the `server status` command to see more detailed information including the arguments used previously to start/stop the server. 

## Forgetting Servers
If you want to wipe all configuration, logs, and WEB-INF files for a server, use the `server forget` command.  This will also remove any administrator settings you may have saved including data sources, mail servers, and server mappings.

```bash
server forget
```

You can forget all your servers at once too if you want to start with a clean slate. 

```bash
server forget --all
```



