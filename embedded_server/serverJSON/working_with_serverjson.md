
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
