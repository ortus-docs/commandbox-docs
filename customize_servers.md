# Customize Servers

The CommandBox server is very lightweight and minimal but should still give you the options you need to set up most local development servers quickly.  To see a list of all the parameters you can pass to the `server start` command, refer to the [CommandBox API Docs](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/commands/server/start.html) or run `server start help` command directly from the CLI.

It's important to note that each server you start opens a new Java process on your host operating system.  This allows each server to have it's own settings and configuration independent of any other server.  Start as many as you need and simply stop them when you're done.  Just keep in mind each server gets its own heap space so keep an eye on your available RAM.

The embedded server instances are also a separate process from the actual CommandBox CLI process, which also runs on Java.  This means you can open the interactive shell, start a few servers, then quit the shell, yet the servers will still continue to run.  You can use the CLI to issue a `stop` command in the servers web root, or just right click on the tray icon.  

There is currently no way to have servers start automatically when your computer boots, though there's nothing preventing you from setting up a local script to run the start commands for you.

## Changing the Port
The `start` command will scan your system and find a random port that is not currently in use to start the server on.  This ensures that multiple embedded servers can run at the same time 
