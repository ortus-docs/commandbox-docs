# Resource Manager

CommandBox uses a custom resource manager in Undertow to resolve "real" paths (relative) in the servlet context with the path on the file system (absolute).  This includes log concerning where the root of the WAR is, where the CF engine web root is, and any web aliases.  The resource manager is responsible for not only resolving paths to CF files, but also static files being sent by CommandBox's web server.

## Cache Servlet Paths

You can activate a cache for all file path resolution in the resource manager by activating this setting.  This is only for production and will eliminate repeated file system hits by your CF engine, such as checking for an Application.cfc file on every request, or testing where the servlet context root is.  This will NOT affect any of the CFML `fileRead()` sort of commands or any code in the CF engine that bypasses the `ServletContext` and directly hits the file system.  The default is `false` when the `profile` is not production.

<pre class="language-bash"><code class="lang-bash"><strong># enable the cache static file cache
</strong><strong>server set web.fileCache.enable=true
</strong><strong>
</strong><strong># Max size of individual files to cache
</strong>server set web.fileCache.maxFileSizeKB=2048

# total size of memory cache
server set web.fileCache.totalSizeMB=50000
</code></pre>

{% code title="server.json" %}
```javascript
{
  "web" : {
    "fileCache" : {
      enable=true,
      maxFileSizeKB=2048,
      totalSizeMB=50000
    }
  }
}
```
{% endcode %}

Standard Adobe ColdFusion installations have a similar cache of "real" paths from the servlet context that is tied to a setting in the administrator called "Cache Webserver paths" but that setting is not available and does not work on CommandBox servers for some reason.

If you enable the cache, but set the max size to `0` that will cache the file system lookup still (like Adobe's setting) but will not cache the actual contents of files.

## Disable Resource Manager Change Listener

There is an XNIO file system watcher started for the web root and any virtual directories in your server. This change listener serves two purposes:

* Cache invalidation for the servlet path lookup matching
* Cache invalidation for welcome file lookups

In normal operations you should have no issues with this, but it has been observed starting a server in a web root with a very large number of files (like over 200,000) can consume a lot of resources, and even cause out of memory errors.  The change listener in Undertow is disabled by default and can be enabled with this setting:

<pre class="language-bash"><code class="lang-bash"><strong>server set web.fileCache.fileSystemWatcherEnable=true
</strong></code></pre>

{% code title="server.json" %}
```javascript
{
  "web" : {
    "fileCache" : {
      fileSystemWatcherEnable=true
    }
  }
}
```
{% endcode %}

## Send File Capability

Some servers (typically Linux) have a "send file" capability which allows a program to use the kernel to directly transfer a file across a port without needing to transfer all the bytes of that file through the application's memory space.  Java is capable of tapping into this behavior via the `FileChannel` class and it has great performance boost when serving up large files via the web server, which now no longer need to have all the bytes of the file read into Java's heap.&#x20;

CommandBox will enable send file capability by default, but it's dependant on your server operating system and file system as to whether it will kick in.  You can control the threshold for how big a file needs to be before Undertow will use the send file capabilities.

```bash
server set web.sendFileMinSizeKB=2048
```

Which creates the following config:

{% code title="server.json" %}
```json
{
  "web" : {
    "sendFileMinSizeKB" : 2048
  }
}
```
{% endcode %}

You can disable the send file feature entirely by setting `web.sendFileMinSizeKB` to `-1`.

## Enable Logging

If you enable a `--debug` or `--trace` server start, it is possible to get additional low-level information in the logs about every single file path that is resolved through the servlet context's resource manager.  This logging is disabled by default since it is quite verbose.

```bash
server set web.resourceManagerLogging=true"
```

Or in the `server.json` as:

{% code title="server.json" %}
```javascript
{
  "web" : {
    "resourceManagerLogging" : "true"
  }
}
```
{% endcode %}

{% hint style="info" %}
For [Multi-Site](../multi-site-support/), Resource Manager Logging can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.
{% endhint %}
