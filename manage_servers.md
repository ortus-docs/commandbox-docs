# Managing CommandBox Servers

CommandBox stores information about each of the servers you've ever started inside `~/.CommandBox/servers.json` so it can remember settings from one run to the next.  You can see a snapshot of this information with the `server status` command.  

```bash
CommandBox> server status

name: myApp
  status: stopped
  webroot: C:\sandbox\myApp
  logdir: C:\Users\Brad\.CommandBox\server\myApp\log
  port: 80
  host: 127.0.0.1
  stopsocket: 12966
  debug: false
  ID: 83B74C2
```

>**Note** : If the server is killed by an outside process other than the `stop` command, CommandBox will still think it's running.  Use the `--force` flag next time you start it.

You can take a quick look at the what's been happening with the `server log` command.  

## Forgetting Servers
If you want to wipe all configuration, logs, and WEB-INF files for a server, use the `server forget` command.  This will also remove any administrator settings you may have saved including data sources, mail servers, and server mappings.

```bash
server forget
```

You can forget all your servers at once too if you want to start with a clean slate. 

```bash
server forget --all
```



