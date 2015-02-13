# Embedded Server

These properties affect how the embedded server starts.  Note, a future release of CommandBox will most likely separate these out into another JSON file for organization.

## defaultPort

**number**

This is the HTTP port the server will be started on when you use the `start` command. Specifying the `port` parameter on the `start` command will override this.  

```bash
package set defaultPort=8080
package show defaultPort
```

There is currently no way to specify a default SSL port for the embedded server.

## engines

**array of objects**

This represents a list of CF engines your project supports and their version with a semvar range.

```javascript
"engines" : [
    { "type" : "lucee", "version" : ">=4.5.x" },
    { "type" : "adobe", "version" : ">=10.0.0" }
]
```

```bash
package set engines="[ { type : 'lucee', version : '>=4.5.x' } ]" --append
package show engines
```

## defaultEngine

**string**


```bash
package set 
package show 
```