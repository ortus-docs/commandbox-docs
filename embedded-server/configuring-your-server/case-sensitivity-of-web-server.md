# Case Sensitivity of Web Server

By default, the web server in CommandBox will follow the case sensitivity of the underlying file system.   So, when on Windows `/FiLe.TxT` will still load an actual file called `/file.txt`.  But on Linux, the case in the browser would need to match that of the file system.  CommandBox allows you to force case sensitivity to be ON or OFF for a server, overriding the server's file system. &#x20;

Note, this controls all access of static file, such as images, js, css, txt, etc as well as the initial lookup of .cfm files.  It will also affect all internal usage of `ServletContext.getRealPath( "/myPath.txt" )`.  It will NOT affect CFML file operations such as `<CFFile>`, `<CFDirectory>`, `fileRead()`, `directoryList()`, etc.  It will also not affect core CF engine functionality such as creating CFCs or cfincluding CFM templates.  Any code directly accessing the file system will use the file systems case sensitivity.  And creating CFC instance is never case sensitive in CFML.

There is a new setting called `web.caseSensitivePaths` which has 3 possible values: `true`, `false`, or an empty string (default).

### Forcing Case sensitivity

To force CommandBox's web server to be case sensitive, even on operating systems like Windows, use the following setting.  There is a nominal performance benifit in doing this and can allow a Windows CommandBox server to mimic a Linux server for testing.

```bash
server set web.caseSensitivePaths=true
```

{% code title="server.json" %}
```javascript
{
  "web" : {
    "caseSensitivePaths" : "true"
  }
}
```
{% endcode %}

### Forcing Case Insensitivity

To force CommandBox's web server to be case insensitive, even on operating systems like Linux, use the following setting.  There is a nominal performance overhead in doing this and can allow a Linux CommandBox server to mimic a Windows IIS server.  In this mode, CommandBox will use an internal cache of file system lookups to improve performance.  If there are two files of the same name using different case, then you will get whatever file is found first.

```bash
server set web.caseSensitivePaths=false
```

{% code title="server.json" %}
```javascript
{
  "web" : {
    "caseSensitivePaths" : false
  }
}
```
{% endcode %}

{% hint style="info" %}
For [Multi-Site](../multi-site-support/), web server case sensitivity can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.
{% endhint %}

