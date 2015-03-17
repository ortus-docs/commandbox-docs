# Server Port and Host

The `start` command will scan your system and find a random port that is not currently in use to start the server on.  This ensures that multiple embedded servers can run at the same time on the same host without collisions.  Ensure any redirects in your applications take the port into account.   

## Any Port in the Storm

You may want to set a specific port to use-- even port 80 if nothing else is using it.  Pass the HTTP port parameter to the start command like so:

 ```bash
 start port=8080
 ```

It is also possible to save the default port in your `box.json`.  Add a `defaultPort` property, or issue the following command:

```bash
package set defaultPort=8080
```
Now every time you `start` your server, the same port will be used.
 
 If the server won't start or is unreachable, make sure it's port is free with your operating system's `netstat` command.  On Unix-based OS's:
 
 ```bash
 $> netstat -pan | grep 80
 ```
 
 ## A Gracious Host
 
 Your application may rely on a specific host name other than the default of `127.0.0.1`.  You can set the host to anything you like, but you must had a `host` file entry that resolves your host name to an IP address assigned to your network adapter (usually 127.0.0.1)
 
```bash
 start host=mycoolsite.local
 ```
 
 If you have multiple IP addresses assigned to your PC, you can bind the server to a specific IP using the `host` parameter.
 
 ```bash
 start host=192.168.10.15 port=80
 ```
 
 
 
 
 
 
 
 
 
 