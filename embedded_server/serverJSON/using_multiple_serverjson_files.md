# Using Multiple server.json Files

With the advent of [Multi-server functionality](../multi-engine_support.md), you may want to regularly start up the same web site with  different settings (such as different CF engine's).  To help with this, you can have more than one JSON file.  The default server configuration file is `server.json`, but you can actually call the file anything you want as long as you use the file's path (or unique name) when starting the server.

Let's say we want to test our app in Lucee 4, Lucee 5 and Adobe 2016.  Let's start 3 servers.  Note we give each server a unique name.  This will come in handy when we want to start/stop the servers by name later.


