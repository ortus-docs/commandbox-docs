# Server.json

Every time you start a server, the settings used to start it are saved in a `server.json` file in the web root. Any parameters that aren't supplied to the `start` command are read from this file \(if it exists\) and used as defaults. Here are the possible properties for a `server.json` file:

`/server.json`

```javascript
{
    "name": "",
    "openBrowser": true,
    "openBrowserURL" : "http://localhost/admin/login",
    "startTimeout": 240,
    "stopsocket": 50123,
    "debug": false,
    "trace": false,
    "console": false,    
    "trayEnable": true,
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
        "javaHome" : "/path/to/java/home"
    },
    "web": {
        "host": "127.0.0.1",
        "webroot": "src/cfml",
        "directoryBrowsing": true,
        "accessLogEnable": true,
        "maxRequests":30,
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
        "HTTP": {
            "enable": true,
            "port": 8080
        },
        "SSL": {
            "enable": false,
            "port": 443,
            "certFile": "",
            "keyFile": "",
            "keyPass": ""
        },
        "AJP": {
            "enable": false,
            "port": 8009
        },
        "rewrites": {
            "enable": true,
            "logEnable": true,
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
        "serverHomeDirectory": "",
        "sessionCookieSecure": true,
        "sessionCookieHTTPOnly": true
    },
    "runwar": {
        "args": "",
        "XNIOOptions": {
            "WORKER_NAME": "MyWorker"
        },
        "UndertowOptions": {
            "ALLOW_UNESCAPED_CHARACTERS_IN_URL": true
        }
    }
}
```

