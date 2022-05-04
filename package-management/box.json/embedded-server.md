# Embedded Server

These properties affect how the embedded server starts. These settings are now deprecated in favor of the new server.json file.

## defaultPort

**number**

This is the HTTP port the server will be started on when you use the `start` command. Specifying the `port` parameter on the `start` command will override this.

```bash
package set defaultPort=8080
package show defaultPort
```

This setting is deprecated in favor of the `port` property of `server.json`. CommandBox will use this setting still if there is no port in server.json and a port argument is not specified with the `start` command.

## engines

**array of objects**

This represents a list of CF engines your project supports and their version with a semvar range.

```javascript
"engines" : [
    { "type" : "railo", "version" : ">=4.2.1" },
    { "type" : "adobe", "version" : ">=10.0.0" }
]
```

```bash
package set engines="[ { type : 'lucee', version : '>=4.5.x' } ]" --append
package show engines
```

_This data is informational only and not yet used by the embedded server_

## defaultEngine

**string**

The default CF engine for the `start` command to use.

```bash
package set defaultEngine=lucee
package show defaultEngine
```

_This data is informational only and not yet used by the embedded server_
