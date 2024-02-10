# Cache Servlet Paths

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
