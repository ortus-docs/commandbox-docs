# Server.json

Every time you start a server, the settings used to start it are saved in a `server.json` file in the web root. Any parameters that aren't supplied to the `start` command are read from this file (if it exists) and used as defaults. Here are the possible properties for a `server.json` file:

`/server.json`

```javascript
{
    "name": "",
    "openBrowser": true,
    "openBrowserURL": "http://localhost/admin/login",
    "startTimeout": 240,
    "stopsocket": 50123,
    "debug": false,
    "trace": false,
    "console": false,
    "profile": "prod",
    "dockEnable": true,
    "trayEnable": true,
    "trayicon": "/path/to/trayicon.png",
    // Used for host-updater module, but not for bindings
    "hostAlias": "site.com",
    "env": {
        "ANYTHING_HERE": "you want",
        "THESE_ARE_ADDED": "As environment variables to the server"
    },
    "trayOptions": [
        {
            "label": "Foo",
            "action": "openbrowser",
            "url": "http://${runwar.host}:${runwar.port}/foobar.cfm",
            "disabled": false,
            "image": "/path/to/image.png"
        },
        {
            "label": "Open VScode",
            "action": "runAsync",
            "command": "code .", // Command is run relative to webroot, for box commands begin command with `box`
            "disabled": false,
            "image": "/path/to/image.png"
        },
        {
            "label": "Open Webroot",
            "action": "openfilesystem",
            "path": "./", // Path is relative to your webroot
            "disabled": false,
            "image": "/path/to/image.png"
        }
    ],
    "jvm": {
        "heapSize": 512,
        "minHeapSize": 256,
        "args": [
            // Can be a string or an array
            "-XX:+UseG1GC",
            "-XX:-CreateMinidumpOnCrash",
            "--add-opens=java.base/java.net=ALL-UNNAMED"
        ],
        "javaHome": "/path/to/java/home",
        "javaVersion": "openjdk11"
    },
    "web": {
        "host": "127.0.0.1",
        // Used for default bindings
        "hostAlias": "site.com",
        "webroot": "src/cfml",
        "directoryBrowsing": true,
        "accessLogEnable": true,
        "maxRequests": 30,
        "gzipEnable": true,
        "gzipPredicate": "regex( '(.*).css' ) and request-larger-than( 500 )",
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
        // DEPRECATED: Use new bindings object
        "HTTP": {
            "enable": true,
            "port": 8080
        },
        // DEPRECATED: Use new bindings object
        "SSL": {
            "enable": false,
            "port": 443,
            "certFile": "",
            "keyFile": "",
            "keyPass": ""
        },
        // DEPRECATED: Use new bindings object
        "AJP": {
            "enable": false,
            "port": 8009
        },
        "bindings": {},
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
        },
        "rules": [
            "path-suffix(/box.json) -> set-error(404)",
            "path-prefix(.env) -> set-error(404)",
            "path-prefix(/admin/) -> ip-access-control(192.168.0.* allow)",
            "path(/sitemap.xml) -> rewrite(/sitemap.cfm)",
            "disallowed-methods(trace)"
        ],
        // 3 ways to specify rulesFile:
        "rulesFile": "../secure-rules.json",
        // Or...
        "rulesFile": [
            "../security.json",
            "../rewrites.json",
            "../app-headers.json"
        ],
        // Or...
        "rulesFile": "../rules/*.json",
        "blockCFAdmin": false,
        "blockSensitivePaths": true,
        "blockFlashRemoting": true
    },
    "sites": {
        "site-name-here": {
            // Any valid option from "web" plus "profile"
        },
        "another-site-name-here": {
            // Any valid option from "web" plus "profile"
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
        "sessionCookieHTTPOnly": true,
        "webXMLOverride": "path/to/web.xml",
        "webXMLOverrideForce": false
    },
    "runwar": {
        "jarPath": "/path/to/runwar.jar",
        "args": [
            // Can be a string or an array
            "--runwar-option",
            "value",
            "--runwar-option2",
            "value2"
        ],
        "XNIOOptions": {
            "WORKER_NAME": "MyWorker"
        },
        "UndertowOptions": {
            "ALLOW_UNESCAPED_CHARACTERS_IN_URL": true
        }
    }
}
```
