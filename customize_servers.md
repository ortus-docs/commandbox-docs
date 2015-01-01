# Customize Servers




## Any Port in the Storm

The `start` command will scan your system and find a random port that is not currently in use to start the server on.  This ensures that multiple embedded servers can run at the same time on the same host without collisions.  Ensure any redirects in your applications take the port into account.   

You may want to set a specific port to use-- even port 80 if nothing else is using it.  Pass the HTTP port parameter to the start command like so:
 
 ```bash
 start 8080
 ```
 

 
 ## A Gracious Host
 
 Your application may rely on a specific host name other than the default of `127.0.0.1`.  
 
 
 
 
 
 
 
