# Basic Authentication

CommandBox's web server supports enabling Basic Auth on your sites.  

```
server set web.basicAuth.enabled=true
server set web.basicAuth.users.brad=pass
server set web.basicAuth.users.luis=pass2
```

That will create the following data in your `server.json`, which will be picked up the next time you start your server.

```json
{
    "web":{
        "basicAuth":{
            "users":{
                "brad":"pass",
                "luis":"pass2"
            },
            "enabled":"true"
        }
    }
}
```