# Configuring Your Server

CommandBox allows you full control over the servers you start.  This includes the port and host, custom JVM args, URL rewriting, web aliases, custom error pages, and custom welcome files.  Configuration can be set at 3 different levels:

1. Passed as parameters to the `server start` command
2. Stored in a `server.json` file for that server
3. Stored in config settings for all servers

Settings will be used in that order.  Also, any parameters passed to the `start` command will automatically be saved to your `server.json` file unless you pass the `--noSaveSettings` flag.
