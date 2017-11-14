# Configuring Your Server

CommandBox allows you full control over the servers you start.  This includes the port and host, custom JVM args, URL rewriting, web aliases, custom error pages, and custom welcome files.  

## Ways to provide configuration

Configuration can be set at several different levels:

1. Passed as parameters to the `server start` command
2. Stored in a `server.json` file for that server
3. Global defaults in the `server.defaults` config setting
4. Internal defaults from the `ServerService`

Settings will be used in that order.  Also, any parameters passed to the `start` command will automatically be saved to your `server.json` file unless you pass the `--noSaveSettings` flag.

## File path handling

A lot of settings used to start a server involve file paths.  Paths starting with a drive letter like `C:/`, a UNC network path like `\\`, or a leading slash like `/` are considered absolute paths and will not be expanded.  Try to avoid absolute paths if you want to make your server config portable.  

Paths that start with a file/folder name like `foo/bar.jar` or `../../lib/my.jar` are relative and the root folder that they are relative to depends on where there are specified.

* If the path is passed as a parameter to the start command, the path is relative to the current working directory
* If the path is in the `server.json` file, it is relative to the folder containing the JSON file.  (Remember the `server.json` doesn't have to be in the web root!)
* If the path is in a global `server.defaults` config setting, it is relative to the web root of the server. 

