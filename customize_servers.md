# Customize Servers



## Process Management

It's important to note that each server you start opens a new Java process on your host operating system.  This allows each server to have it's own settings and configuration independent of any other server.  Start as many as you need and simply stop them when you're done.  Just keep in mind each server gets its own heap space so keep an eye on your available RAM.

The embedded server instances are also a separate process from the actual CommandBox CLI process, which also runs on Java.  This means you can open the interactive shell, start a few servers, then quit the shell, yet the servers will still continue to run.  You can use the CLI to issue a `stop` command in the servers web root, or just right click on the tray icon.  

There is currently no way to have servers start automatically when your computer boots, though there's nothing preventing you from setting up a local script to run the start commands for you. 

## Any Port in the Storm

The `start` command will scan your system and find a random port that is not currently in use to start the server on.  This ensures that multiple embedded servers can run at the same time on the same host without collisions.  Ensure any redirects in your applications take the port into account.   

You may want to set a specific port to use-- even port 80 if nothing else is using it.  Pass the HTTP port parameter to the start command like so:
 
 ```bash
 start 8080
 ```
 

 
 ## A Gracious Host
 
 Your application may rely on a specific host name other than the default of `127.0.0.1`.  
 
 
 
 
 
 
 
