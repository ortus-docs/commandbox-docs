# Ad-hoc Env Vars

There are many ways to pass environment variables into a server.  You can set them in your actual OS shell before starting box, set them as CommandBox env vars, use the `commandbox-dotenv` module to read them from a `.env` file or many Docker-based options.  Just in case you wanted one MORE way to do it, you can also set env vars directly in your `server.json` file. &#x20;

```javascript
{
    "name" : "My server",
    "env":{
        "brad":"wood",
        "cfconfig":{
            "requestTimeout":"1,2,3,4"
        },
        "cfconfig_secureJSON" : false,
        "luis":{
            "majano":"${brad}"
        },
        "myPort":"1234"
    },
    "web":{
        "HTTP":{
            "port":"${myPort}"
        }
    }
}
```

These env vars will not be visible in the CommandBox shell like a `.env` file is, but will be used exclusively when starting the server.  They will be visible inside the running server and also present in CommandBox to any interception points or CFConfig while the server is starting.

Note the two CFConfig examples above do the same thing (assuming you have `commandbox-cfconfig` installed.  Any nested keys will be concatenated with underscores when the actual env var is set.  Therefore the `cfconfig` key and nested `requestTimeout` key will create an env var actually called `cfconfig_requestTimeout` when the server starts.  This is simply a convenience to help you group together related env vars with a matching prefix.

You can see that the `web.http.port` is using an env var from the `env` block above it.  All env vars in the `env` block will be added to CommandBox's environment BEFORE the rest of the JSON file is expanded.  Env vars in the `env` block can ALSO reference other pre-existing env vars.  In the example above, `${brad}` is assuming there is a pre-existing env var of that name set outside this file.

## Just for kicks

In case anyone is wondering, it's actually possible to override your `server.json` settings from inside the `env` block like so:

```javascript
{
    "env":{
        "box":{
            "server":{
                "web":{
                    "HTTP":{
                        "port":"8080"
                    }
                }
            }
        },
        "box_server_web_SSL_port" : "1443"
    },
    "web":{
        "HTTP":{
            "enable":true
        }
    }
}
```

but we can't imagine why you'd ever want to do that :) That would be the equivalent of setting the following env vars outside of CommandBox:

```bash
box_server_web_HTTP_port=8080
box_server_web_SSL_port=1443
```

