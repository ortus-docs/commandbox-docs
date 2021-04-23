# Single Server Mode

CommandBox is designed to allow you to start, stop, and manage as many servers as you like.  However, when using CommandBox in an environment such as Docker containers where you only ever want to have a single server, this can cause issues where warming up the server with different names creates more than one server.  You can enable **Single Server Mode** which will only allow CommandBox to have a single server that shows when you run the `server list` command.

```bash
config set server.singleServerMode=true
```

This is a global setting that takes immediate effect.  If you already have more than one server defined, it will not remove the extra ones, it will simply re-use the first server it finds.  In this mode, the server name becomes essentially meaningless.  Each of these commands would use the same default server

```bash
start name=foo
stop name=bar
restart name=brad
```



