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
    trayOptions : [],
	jvm : {
		heapSize : 512,
		args : ''
	},
	web : {
		host : '127.0.0.1',
        webroot : 'src/cfml'
		directoryBrowsing : true,
        aliases : {},
        errorPages : {},
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


