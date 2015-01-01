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

