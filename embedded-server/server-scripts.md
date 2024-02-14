# Server Scripts

Server scripts work just like [Package Scripts](../package-management/package-scripts.md), but they only apply to server-related interceptor points, and they go in your `server.json`. If you have several servers in a folder with their own `server-name.json` files, the server scripts can be different per server.

The interception points which will fire a server script are:

* `preServerStart`
* `onServerStart`
* `onServerInstall`
* `onServerInitialInstall`
* `onServerStop`
* `preServerForget`
* `postServerForget`

Read more about when these interception points fire [here](../developing-for-commandbox/interceptors/core-interception-points/).

Configure server scripts like so in your `server.json`:

```json
{
    "scripts":{
        "onServerInstall":"cfpm list"
    }
}
```

### Ad-hoc server scripts

Just like package scripts, you can also create ad-hoc scripts for a given server. They are executed with the `server run-script` command. Define them as additional keys in the `scripts` block.

```json
{
    "name" : "My Server"
    "scripts":{
        "myScript":"server log --follow"
    }
}
```

And run them like so:

```
server run-script myScript
```

### Running multiple commands

You can run several commands into a single one using `&&`.  You can also do this by specifying an array of strings instead of a string like so:

```json
{
  "name" : "My Server",
  "scripts" : {
      "updateCode" : [
          "server stop",
          "!git pull",
          "server start"
       ],
  }
}
```

This can be much more readable for multiple commands.  Note, this is functionality equivalent to using `&&`, which means any erroring command will stop execution.

