# Experimental Features

There are a few experimental features in Runwar that are available for your servers. &#x20;

## Resource Manager Logging

CommandBox uses a custom resource manager in Undertow to resolve "real" paths (relative) in the servlet context with the path on the file system (absolute).  This includes log concerning where the root of the WAR is, where the CF engine web root is, and any web aliases.  If you enable a `--debug` or `--trace` server start, it is possible to get additional low-level information in the logs about every single file path that is resolved through the servlet context's resource manager.  This logging is disabled by default since it is quite verbose.

```bash
server set runwar.args="--resource-manager-logging=true"
```

Or in the `server.json` as:

{% code title="server.json" %}
```javascript
{
  "runwar" : {
    "args" : "--resource-manager-logging=true"
  }
}
```
{% endcode %}



## Cache Servlet Paths

You can activate a cache for all file path resolution in the resource manager by activating this setting.  This is only for production and will eliminate repeated file system hits by your CF engine, such as checking for an Application.cfc file on every request, or testing where the servlet context root is.  This will NOT affect any of the CFML `fileRead()` sort of commands or any code in the CF engine that bypasses the `ServletContext` and directly hits the file system.  The default is `false`.

```bash
server set runwar.args="--cache-servlet-paths=true"
```

{% code title="server.json" %}
```javascript
{
  "runwar" : {
    "args" : "--cache-servlet-paths=true"
  }
}
```
{% endcode %}

Standard Adobe ColdFusion installations have a similar cache of "real" paths from the servlet context that is tied to a setting in the administrator called "Cache Webserver paths" but that setting is not available and does not work on CommandBox servers for some reason..

## Custom Log Pattern

The Java logging library Log4j is used for servers' log files and console logs.  The default logging pattern is:

```
[%-5p] %c: %m%n
```

You can customize this with any valid Log4j pattern layout, which you can find here:

{% embed url="https://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/PatternLayout.html" %}

This would put the date/time into every log message:

```
server set runwar.args='--log-pattern "[%-5p] %d{dd MMM yyyy HH:mm:ss.SSS} %c: %m%n"'
```

This would log ONLY the message with no severity or category:

```
server set runwar.args='--log-pattern "%m%n"'
```

Note, the color coding of log lines in CommandBox is dependent upon the default Log4j pattern layout.

## Disable Resource Manager Change Listener

There is an XNIO file system watcher started for the web root and any virtual directories in your server. This change listener serves two purposes:

* Cache invalidation for the servlet path lookup matching
* Cache invalidation for welcome file lookups

In normal operations you should have no issues with this, but it has been observed starting a server in a web root with a very large number of files (like over 200,000) can consume a lot of resources, and even cause out of memory errors.  The change listener in Undertow can be disabled with this setting:

```
server set runwar.args='--resource-manager-file-system-watcher=false'
```
