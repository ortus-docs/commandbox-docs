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

I can control the name of the JSON files by using the `serverConfigFile` parameter, but when CommandBox sees us use the `name` parameter, it will automatically create a file called `server-{name}.json`.  In this case, we'll have 3 new files:

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
server show server-lucee5.json name
# named args are required to set properties
server set serverConfigFile=server-lucee5.json jvm.heapSize=1024 
server clear server-lucee5.json jvm
```
>**Info** The property name and server config file path are interchangeable for the `server show` and `server clear` commands for your convenience.


