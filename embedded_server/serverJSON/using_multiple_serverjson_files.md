# Using Multiple server.json Files

With the advent of [Multi-server functionality](../multi-engine_support.md), you may want to regularly start up the same web site with  different settings (such as different CF engine's).  To help with this, you can have more than one JSON file.  

## Create `anything.json`

The default server configuration file is `server.json`, but you can actually call the file anything you want as long as you use the file's path (or unique name) when starting the server.

Let's say we want to test our app in Lucee 4, Lucee 5 and Adobe 2016.  Let's start 3 servers.  Note we give each server a unique name.  This will come in handy when we want to start/stop the servers by name later.

```bash
start cfengine=lucee@4 name=lucee4
start cfengine=lucee@5 name=lucee5
start cfengine=adobe@2016 name=adobe2016
```

>**Info** It's important to always use a name when starting more than one server.  Otherwise, the settings will override each other and only the last server will be saved.  Also, you will only be able to stop the last server via the `stop` command.

You can control the name of the JSON files by using the `serverConfigFile` parameter, but when CommandBox sees us use the `name` parameter, it will automatically create a file called `server-{name}.json`.  In this case, we'll have 3 new files:

**server-lucee4.json**
```js
{
    "app":{
        "cfengine":"lucee@4"
    },
    "name":"lucee4"
}
```
**server-lucee5.json**
```js
{
    "app":{
        "cfengine":"lucee@5"
    },
    "name":"lucee5"
}
```

**server-adobe2016.json**
```js
{
    "app":{
        "cfengine":"adobe@2016"
    },
    "name":"adobe2016"
}
```

## Interacting with non-standard JSON file names

If you run the `server show` command, you'll see it returns `{}`.  This is because it looks for a file called `server.json` by default.  Not to worry, you can still programmatically manipulate your JSON files like so:

``` bash
# Show all properties
server show server-lucee5.json

# Show one property
server show server-lucee5.json name

# named args are required to set properties
server set serverConfigFile=server-lucee5.json jvm.heapSize=1024 

# Clear properties
server clear server-lucee5.json jvm
```
>**Info** The property name and server config file path are interchangeable for the `server show` and `server clear` commands for your convenience.

## Use server JSON files to control servers
Now that you have 3 JSON files-- one for each server, you can use the path to the JSON file (absolute or relative to your CWD) to control each server.

```bash
start serverConfigFile=server-lucee4.json
```

For your convenience, if you pass in a path to an existing JSON for the server name, we'll use it as the  `serverConfigFile` parameter.  

```bash
start server-lucee4.json
stop server-lucee4.json
start server-adobe2016.json
```

This trick works on any `server` commands
```
# Open your Lucee 5 site in the browser
server open server-lucee5.json

# cd into the web root for your Adobe 2016 web
server cd /path/to/server-adobe2016.json
```

## Use server names to control servers
After you've started a server at least once, you can use its server name to control it as well which is a great shortcut.  CommandBox will recognize the server name and remember where the server JSON for that server name is stored. Then it will pull the correct web root from the JSON file.

```bash
start lucee4
start lucee5
start adobe2106

restart adobe2016
stop lucee4
```