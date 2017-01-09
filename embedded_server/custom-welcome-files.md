# Custom Welcome Files

The default welcome files are the usual index.cfm, index.htm, index.html, etc but you can override this with the `welcomeFiles` setting in your `server.json` by providing a comma-delimited list of files that you would like CommandBox to look for when a user hits a directory on a running server.

```
server set web.welcomeFiles='go.cfm,main.cfm,index.html'
```

This setting is a complete override of the defaults, so you need to specify the full list.