# server.json

Every time you start a server, the settings used to start it are saved in a `server.json` file in the web root.  Any parameters that aren't supplied to the `start` command are read from this file \(if it exists\) and used as defaults.  Here are the possible properties for a `server.json` file:

`/server.json`

```javascript
{
    "name": "",
    "openBrowser": true,
    "openBrowserURL" : "http://localhost/admin/login",
    "startTimeout": 240,
    "stopsocket": 0,
    "debug": false,
    "trace": false,
    "console": false,
    "trayicon": "/path/to/trayicon.png",
    "trayOptions": [
        {
            "label": "Foo",
            "action": "openbrowser",
            "url": "http://${runwar.host}:${runwar.port}/foobar.cfm",
            "disabled": false,
            "image": "/path/to/image.png"
        }
    ],
    "jvm": {
        "heapSize": 512,
        "minHeapSize": 256,
        "args": "",
        "javaHome" : "/path/to/java/home",
    },
    "web": {
        "host": "127.0.0.1",
        "webroot": "src/cfml",
        "directoryBrowsing": true,
        "aliases": {
            "/foo": "../bar",
            "/js": "C:/static/shared/javascript"
        },
        "errorPages": {
            "404": "/path/to/404.html",
            "500": "/path/to/500.html",
            "default": "/path/to/default.html"
        },
        "welcomeFiles": "index.cfm,main.cfm,go.cfm",
        "http": {
            "port": 8080,
            "enable": true
        },
        "ssl": {
            "enable": false,
            "port": 443,
            "certFile": "",
            "keyFile": "",
            "keyPass": ""
        },
        "rewrites": {
            "enable": true,
            "config": "/path/to/config.xml",
            "statusPath": "/rewriteStatus",
            "configReloadSeconds": 60
        },
        "basicAuth": {
            "enable": true,
            "users": {
                "userName1": "password1",
                "userName2": "password2"
            }
        }
    },
    "app": {
        "logDir": "",
        "libDirs": "",
        "webConfigDir": "",
        "serverConfigDir": "",
        "webXML": "",
        "WARPath": "",
        "cfengine": "lucee@5.x",
        "restMappings": "/rest/*,/api/*",
        "serverHomeDirectory": ""
    },
    "runwar": {
        "args": ""
    }
}
```

## Server Setting Order

Settings are loaded in this order:

1. Typed by the user into the `start` command parameters
2. Read from `server.json`
3. Global defaults in the `server.defaults` config setting
4. Internal defaults from the `ServerService`



