# Configuring Sites

As soon as CommandBox flips over to multi-site mode, the settings in the `web` object will become defaults that apply to all sites.  This allows you to group global settings into the top-level `web` object and then override what you need for each site.  Here is the full order of precedence for what settings will be applied:

* settings in a `.site.json` file inside a web root of a site
* settings in an external site JSON file pointed to by the `siteConfigFiles` setting in `server.json`
* site-specific object in the `sites` object of `server.json`&#x20;
* settings in the `web` object of `server.json`
* `server.default` settings in CommandBox's global config settings

### Settings global to a server

Since all sites for a given server do run inside the same JVM, there are some settings which cannot be customized on a per-site basis.  They are as follows:

* JRE/JDK the server runs on&#x20;
* JVM args, heap size&#x20;
* CF Engine/version&#x20;
* Console log&#x20;
* Tuckey Rewrites (part of servlet)&#x20;
* Environment Variables
* Tray icon (there is a single tray icon for the entire server)

### Per-site settings

Everything normally set in the `web` block of your `server.json` can be configured separately for each site.  These settings include:

* GZIp enabled and GZip predicate
* Access log
* Use proxy forwarded IP
* CommandBox Server Rules (Undertow Predicate Language)
  * SSL settings (HSTS, SSL Redirect)
  * Block CF Admin
  * Block flash remoting
  * Block sensitive paths
* Security
  * Basic auth
  * Client cert auth
  * Security predicate
* Custom error pages (404, 500, etc)
* Mime types
* Welcome files
* Allowed file extensions
* Directory browsing
* Aliases/Virtual dirs
* File cache settings
* Case sensitive paths
* Web root
* Server Profile (even though this is not inside the `web` object in your `server.json`, it can still be set in a `sites` block to override for that site.)

And remember, all of the settings in the section above can be defaulted for all sites in the `web` block at the top of your `server.json` and then overridden in the `sites.siteName` block or in a `.site.json` file.

### Debugging settings

There is much-improved console output now coming from Runwar when it server starts up.  Add `--verbose` or `--debug` to your `server start` command and you'll see output like so at the top of the server start in the interactive job output:

```
   |   |--------------------------------------------------------------
   | √ | Configuring site [site1]
   |   |---------------------------------------
   |   | Site name - site1
   |   | Webroot - C:\path\to\site1\
   |   | Site config file - C:\path\to\server.json
   |   |---------------------------------------
   |   | √ | Setting site [site1] Profile to [development]
   |   |   |------------------------------------------------------------
   |   |   | Profile set from server bound to localhost
   |   |   | Block CF Admin disabled
   |   |   | Block Sensitive Paths enabled
   |   |   | Block Flash Remoting enabled
   |   |   | Allowed Extensions: [log]
   |   |   | Directory Browsing enabled
   |   |   | File Caching disabled
   |   |   |------------------------------------------------------------
   | √ | Configuring site [site2]
   |   |---------------------------------------
   |   | Site name - site2
   |   | Webroot - C:\path\to\site2\
   |   | Site config file - C:\path\to\server.json
   |   |---------------------------------------
   |   | √ | Setting site [site2] Profile to [development]
   |   |   |------------------------------------------------------------
   |   |   | Profile set from server bound to localhost
   |   |   | Block CF Admin enabled
   |   |   | Block Sensitive Paths enabled
   |   |   | Block Flash Remoting enabled
   |   |   | Allowed Extensions: [log2]
   |   |   | Directory Browsing enabled
   |   |   | File Caching disabled
   |   |   |------------------------------------------------------------
   | √ | Configuring site [site3]
   |   |---------------------------------------
   |   | Site name - site3
   |   | Webroot - C:\path\to\site3\
   |   | Site config file - C:\path\to\server.json
   |   |---------------------------------------
   |   | √ | Setting site [site3] Profile to [development]
   |   |   |------------------------------------------------------------
   |   |   | Profile set from server bound to localhost
   |   |   | Block CF Admin disabled
   |   |   | Block Sensitive Paths enabled
   |   |   | Block Flash Remoting enabled
   |   |   | Allowed Extensions: [log]
   |   |   | Directory Browsing disabled
   |   |   | File Caching disabled
   |   |   |------------------------------------------------------------
   | √ | Configuring site [default]
   |   |-----------------------------------------
   |   | Site name - default
   |   | Webroot - C:\path\to\default\
   |   | Site config file - C:\path\to\server.json
   |   |-----------------------------------------
   |   | √ | Setting site [default] Profile to [development]
   |   |   |--------------------------------------------------------------
   |   |   | Profile set from server bound to localhost
   |   |   | Block CF Admin disabled
   |   |   | Block Sensitive Paths enabled
   |   |   | Block Flash Remoting enabled
   |   |   | Allowed Extensions: [log]
   |   |   | Directory Browsing enabled
   |   |   | File Caching disabled
   |   |   |--------------------------------------------------------------
```

Furthermore, once the actual server process gets underway, with the `--trace` flag you'll see additional console output like so:

```
[INFO ] Runwar: ******************************************************************************
[INFO ] Runwar: Starting Runwar
[INFO ] Runwar:   - Runwar Version: 5.0.0-SNAPSHOT
[INFO ] Runwar:   - Java Version: 11.0.22+7 (Eclipse Adoptium)
[INFO ] Runwar:   - Java Home: C:\path\to\jre
[INFO ] Runwar: ******************************************************************************
[INFO ] Runwar: Listeners:
[INFO ] Runwar:   - Binding HTTP on 0.0.0.0:80
[DEBUG] Runwar:      Setting HTTP/2 enabled: true
[INFO ] Runwar: ******************************************************************************
[INFO ] Runwar: Configuring Servlet
[DEBUG] Runwar:   File cache is disabled
[DEBUG] Runwar:   Ignoring web.xml welcome file, so adding server options welcome files to deployment manager.
[INFO ] Runwar:   Found WEB-INF: 'C:\path\to\WEB-INF'
[DEBUG] Runwar:   Parsing 'C:\path\to\WEB-INF\web.xml'
[TRACE] Runwar:     Total No. of context-params: 0
[TRACE] Runwar:     Total No. of listeners: 0
[TRACE] Runwar:     Total No. of servlets: 2
[TRACE] Runwar:       servlet-name: CFMLServlet, servlet-class: lucee.loader.servlet.CFMLServlet
[TRACE] Runwar:       servlet-name: RESTServlet, servlet-class: lucee.loader.servlet.RestServlet
[TRACE] Runwar:       Mapping servlet-name: CFMLServlet, url-pattern: *.cfc
[TRACE] Runwar:       Mapping servlet-name: CFMLServlet, url-pattern: *.cfm
[TRACE] Runwar:       Mapping servlet-name: CFMLServlet, url-pattern: *.cfml
[TRACE] Runwar:     Total No. of welcome files: 4
[TRACE] Runwar:       welcome-file: index.cfm
[TRACE] Runwar:       welcome-file: index.lucee
[TRACE] Runwar:       welcome-file: index.html
[TRACE] Runwar:       welcome-file: index.htm
[INFO ] Runwar: ******************************************************************************
[INFO ] Runwar: Creating deployment [default]
[DEBUG] Runwar:   Initialized MappedResourceManager
[INFO ] Runwar:     Web Root: C:\path\to\default
[DEBUG] Runwar:     Aliases: {/js=C:\path\to\javascript}
[DEBUG] Runwar:   Adding Mime types
[TRACE] Runwar:   - log = 'text/plain'
[DEBUG] Runwar:   New servlet context created for [default]
[DEBUG] Runwar: ******************************************************************************
[INFO ] Runwar: Creating deployment [site3]
[DEBUG] Runwar:   Initialized MappedResourceManager
[INFO ] Runwar:     Web Root: C:\path\to\site3
[DEBUG] Runwar:     Aliases: {/js=C:\path\to\javascript, /js-brad=C:\path\to\site3\javascript}
[DEBUG] Runwar:   Adding Mime types
[TRACE] Runwar:   - log = 'text/plain'
[DEBUG] Runwar:   New servlet context created for [site3]
[DEBUG] Runwar: ******************************************************************************
[INFO ] Runwar: Creating deployment [site1]
[DEBUG] Runwar:   Initialized MappedResourceManager
[INFO ] Runwar:     Web Root: C:\path\to\site1
[DEBUG] Runwar:     Aliases: {/js=C:\path\to\javascript}
[DEBUG] Runwar:   Adding Mime types
[TRACE] Runwar:   - log = 'text/plain'
[DEBUG] Runwar:   New servlet context created for [site1]
[DEBUG] Runwar: ******************************************************************************
[INFO ] Runwar: Creating deployment [site2]
[DEBUG] Runwar:   Initialized MappedResourceManager
[INFO ] Runwar:     Web Root: C:\path\to\site2
[DEBUG] Runwar:     Aliases: {/js=C:\path\to\site2\javascript}
[DEBUG] Runwar:   Adding Mime types
[TRACE] Runwar:   - log2 = 'application/xml'
[TRACE] Runwar:   - log = 'text/plain'
```
