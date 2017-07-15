# Debugging Server Starts
If a server isn't starting, the first thing to run is the `server log` command.  It will show you the console log for that server.
```
server log
server log serverName
```
## Tailing and Following logs
If the log is very large, use the `tail` command to just see the last few lines of it.
```
server log | tail
server log | tail lines=100
```
To get a live stream of the console log from a running server, use the `--follow` flag and the command will continue streaming new lines to the console until you press Ctrl-C to stop.
```
server log --follow
```
## Start server in console mode
You can use the `--console` flag to the `server start` command to start a server in the foreground.  The console log will be live-streamed to the CLI and the log will continue streaming as long as the server is running.  Press Ctrl-C to stop the server and stop streaming the log file.
```
server start --console
```
## Debug Logging
You can get additional information about a server start with the `--debug` flag.  When debug is set, the `start` command will not exit immediately, but wit for the server to come up and live stream the debugging information and server logs to the console while the server is coming up.  
```
server start --debug
server start --debug --console
```

## Maximum logging! (trace)
You may still really be having issues getting your server to start up correctly due to a setting not getting picked up, rewrites not working, or maybe a jar not loading.  You can "drink from the firehose" so to speak by turning on `trace` level logging.  This works best when starting the server via the console so you can watch the logging as it streams past.  

```
server start --trace --console
```

