# CommandBox Server Processes

It's important to note that each server you start opens a new Java process on your host operating system.  This allows each server to have it's own settings and configuration independent of any other server.  Start as many as you need and simply stop them when you're done.  Just keep in mind each server gets its own heap space so keep an eye on your available RAM.

## Server Runs Independant of CLI

The embedded server instances are also a separate process from the actual CommandBox CLI process, which also runs on Java.  This means you can open the interactive shell, start a few servers, then quit the shell, yet the servers will still continue to run.  You can use the CLI to issue a `stop` command in the servers web root, or just right click on the tray icon.  

There is currently no way to have servers start automatically when your computer boots, though there's nothing preventing you from setting up a local script to run the start commands for you. 

##WEB-INF
You may be used to having your server's WEB-INF folder living in your web root. The CommandBox embedded server still has a dedicated WEB-INF folder for each server you start, but it lives under the main CommandBox installation directory in your user folder.  Changes you make to the WEB-INF such as adding jars or new tas will be persisted until you issue a `server forget` command.