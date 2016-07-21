# server.json

Every time you start a server, the settings used to start it are saved in a `server.json` file in the web root.  Any parameters that aren't supplied to the `start` command are read from this file (if it exists) and used as defaults.  Here are the possible properties for a `server.json` file:

**`/server.json`**
```javascript
{
	name : '',
	openBrowser : true,
	stopsocket : 0,
	debug : false,
	trayicon : '/path/to/trayicon.png',
	jvm : {
		heapSize : 512,
		args : ''
	},
	web : {
		host : '127.0.0.1',
        webroot : 'src/cfml'
		directoryBrowsing : true,
		http : {
			port : 8080,
			enable : true
		},
		ssl : {
			enable : false,
			port : 443,
			cert : '',
			key : '',
			keyPass : ''
		},
		rewrites : {
			enable : true,
			config : '/path/to/config.xml'
		}
	},
	app : {
		logDir : '',
		libDirs : '',
		webConfigDir : '',
		serverConfigDir :'',
		webXML : '',
		WARPath : '',
		cfengine : 'lucee@5.x'
	},
	runwar : {
		args : ''
	}
}
```

## Server Setting Order

Settings are loaded in this order:

1. Typed by the user into the `start` command parameters
2. Read from `server.json`
3. Global defaults in the `server.defaults` config setting
3. Internal defaults from the `ServerService`

## Packaging Your Server

This allows you to package up an app that requires special start settings such as rewrites, JVM args, or heap size, and anyone can run it with the same settings you do by simply typing `server start`.   Make sure to not deploy the `server.json` file to your production server where it may be web-accessible.

To help with this, you can store your `server.json` file outside of the web root and use the `web.webroot` property in it to point to the location of the web root. This can be an absolute path or a  relative path to the location of the JSON file.

```bash
server set web.webroot=www
```

## Working With Server.json

Interacting with the `server.json` file uses the commands `server set`, `server show`, and `server clear`, which work the same as the `package set/show/clear` commands.

Set the port for your server:
```bash
server set web.http.port=8080 
```

View the port:

```bash
server show web.http.port
```

Remove the saved setting:

```bash
server clear web.http.port
```

For completeness, here is the full list of all `server.json` properties that can be set.  This is a flattened version of the JSON at the top of this page:

```
stopsocket
trayicon
name
debug
openbrowser
jvm.heapsize
jvm.args
web.host
web.webroot
web.directorybrowsing
web.http.port
web.http.enable
web.ssl.cert
web.ssl.keypass
web.ssl.key
web.ssl.port
web.ssl.enable	
web.rewrites.config
web.rewrites.enable
app.webxml
app.logdir
app.serverconfigdir
app.libdirs
app.webconfigdir
app.WARPath
app.cfengine
runwar.args
```