# Multi-Site Support

The default behavior when you run `server start` is to start a server that points only to a single web root.  There are two methods you can use to host more than one web root at a time in a single CommandBox server:

* ModCFML- this requires a web server sitting in front of CommandBox which sends special HTTP headers that instruct CommandBox what to use as the web root.
* Multi-site mode- this is new in CommandBox 6.0 and it allows full configuration of multiple sites (web roots), rewrite, HTTP/SSL bindings, and settings inside of a single server.&#x20;

In both cases, there is a single JVM process and a single CF Engine at play.  So all sites will use the same CF engine and version.  If you want to run different versions of CF, you'll need more than one server.  You can use a reverse proxy in your server rules to still access all your sites via a single "front end" server instance. &#x20;

## Enabling Multi-site mode

There's no flag or setting for enabling multiple sites in a single server.  All you have to do is&#x20;

* define a `sites` object in `server.json`
* or a `siteConfigFiles` setting in `server.json`

inside your `server.json` and if 1 or more sites are found, CommandBox will automatically switch over to "multi-site" mode.   It's easy to tell if a server has more than one site.  When you start it, the console output will have a dedicated section in the output for each site.  There is also additional information in the `server info` command about each site.

```
❯ start --dryRun
 √ | Starting Server
   | √ | Configuring site [site1]
   |   | √ | Setting site [site1] Profile to [production]
   | √ | Configuring site [site2]
   |   | √ | Setting site [site2] Profile to [production]
   | √ | Configuring site [site3]
   |   | √ | Setting site [site3] Profile to [production]
```

There is a repo on Github that contains working examples of all the features of multi-site.  Feel free to skip straight there to learn by example.

[https://github.com/Ortus-Solutions/commandbox-tests](https://github.com/Ortus-Solutions/commandbox-tests)

{% hint style="info" %}
You are free to use CommandBox as you with for development purposes, but if you are using more than 2 sites in production, we ask you consider supporting us with a [CommandBox Pro](https://www.ortussolutions.com/products/commandbox-pro) license which bundles support and commercial modules and extensions.
{% endhint %}

As soon as CommandBox flips over to multi-site mode, the settings in the `web` object will become defaults that apply to all sites.  This allows you to group global settings into the top-level `web` object and then override what you need for each site.  Here is the full order of precedence for what settings will be applied:

* settings in a `.site.json` file inside a web root of a site
* settings in an external site JSON file pointed to by the `siteConfigFiles` setting in `server.json`
* site-specific object in the `sites` object of `server.json`&#x20;
* settings in the `web` object of `server.json`
* `server.default` settings in CommandBox's global config settings.

Let's explore the two methods of defining more sites.

### Define a "sites" object in server.json

The `sites` object will have a key for each site you want to define where the name of the key is the name of each site and the value is another object containing the configuration of that site.  Think of all of the setting which can be set in the `web` object of `server.json`.  Each site object in the `sites` object can have all the same settings as the top-level `web` object so it will be very familiar. &#x20;

{% code title="server.json" %}
```javascript
{
    "name":"My server",
    "web":{
        // These settings apply to all sites
        "accessLogEnable":"true",
        "bindings":{
            "HTTP":{
                "listen":"80"
            }
        }
    },
    "sites":{
        "site1":{
            "hostAlias":"site1.com",
            "webroot":"/path/to/site1"
        },
        "site2":{
            "hostAlias":"site2.com",
            "webroot":"/path/to/site2",
            // Overrides the default above coming from "web"
            "accessLogEnable":"false",
        }
    }
}
```
{% endcode %}

The simple example `server.json` above, some defaults that apply to all sites are defined in the top-level `web` object.  Then we have two sites, `site1` and `site2` defined with their own settings.  Remember, each site object is just a copy of the `web` object above, so all the same settings are valid.  You can use the `server set` command to configure these and rely on the tab completion to help prompt you with all valid options.

The ONLY required settings for each site are

* &#x20;`webroot`
* or `siteConfigFile`  which points to a JSON file on disk where a web root would be required to be defined

```
{
    "sites":{
        "client-1":{
            "siteConfigFile":"/var/www/client1-site.json"
        },
        "client-2":{
            "siteConfigFile":"/var/www/client2-site.json”
        }
    }
}
```

If you want to access each site individually, you'll either want to specify a separate HTTP/SSL/AJP binding OR a custom hostname for each site.  Otherwise, you'll have no way to access them!

You can also place a `.site.json` file inside of the web root, which will be found by convention and its contents will be added to the site object for that site.  This is akin to configuring your sites in Apache, but having an `.htaccess` file in each web root to override settings. &#x20;

System setting expansions will be processed in any `.site.json` files, and if a `.env` file exists in the web root as well, thoses env vars will be loaded, but JUST for that `.site.json` file's expansions.

### Define a `siteConfigFiles` setting in `server.json`

If you have a lot of sites, or a lot of settings, the first option can become unwieldly since the `server.json` will have a lot of stuff in it.  That is why we also allow you to specify a `siteConfigFiles` setting which contains a list or array of file globbing patterns or directories to look for JSON files in.  Once all the JSON files have been found, they will be used to define sites.  each JSON file will contain JUST the contents of the object you would have put in the `sites` object of `server.json` which has all the same options as a `web` object in `server.json`.

Valid examples could be:

{% code title="server.json" %}
```javascript
{
    "name":"My server",
    "web":{
        // These settings apply to all sites
        "accessLogEnable":"true",
        "bindings":{
            "HTTP":{
                "listen":"80"
            }
        }
    },
    "siteConfigFiles" : "C:/absolute/path/to/configs/"
    // or...    
    "siteConfigFiles" : "../relative/path/to/configs/"
    // or...    
    "siteConfigFiles" : "../marketingSites/*-active.json,../internalSites/**/*-active.json"
    // or...    
    "siteConfigFiles" : [
        "../marketingSites/*-active.json",
        "../internalSites/**/*-active.json"
    ]
    // or...    
    "siteConfigFiles" : [
        "../sites/site1.json",
        "../sites/site2.json",
        "../sites/site3.json"
    ]
}
```
{% endcode %}

System setting expansions will be processed in any of the site JSON files located via your file globbing patterns.  If the `server.json` has a `sites` object AND a `siteConfigFiles` setting, all sites will be combined together.  You choose the approach that fits your needs.  An example of a single site JSON&#x20;

## Bindings

HTTP bindings are much more robust in multi-site mode.  In single-site mode, there is basically no such thing as host name bindings.  All traffic coming into that CommandBox server is simply routed to the one and only server regardless of its host name or IP address. &#x20;

A command server can now have

* Any number of HTTP bindings on any IP/port combination
* Any number of SSL bindings on any IP/port combination
* Any number of AJP bindings on any IP/port combination

And furthermore, a binding can be shared for all sites on the server, or could serve traffic to ONLY a single site depending on how you configure it.  Think how IIS, Nginx or Apache work.  You now have the same level of flexibility in CommandBox!

CommandBox 6 has introduced a new syntax for bindings (and is still backwards compat with the old JSON syntax too) that allows you to specify hostnames alongside the bindings.

{% code title="server.json" %}
```json
{
  "web":{
    "bindings":{
      "HTTP":{
  
      "listen":"80",
      // or...
      "listen":"*:80",
      // or...
      "listen":"0.0.0.0:80",
      // or...
      "listen":"10.10.0.123:80",
      
      // same as ""
      "host" : "*",
      // or...
      "host":"www.domain.com,www.something.com",
      // or...
      "host": [
        "www.domain.com",
        "www.something.com"
      ]  
  
    }
  }
}
```
{% endcode %}

Even though each site can use a different IP/port, a very common setup is to have a single port 80/443 binding at the server level shared by all sites, and then simply specify a hostname for each site.  If you only need to specify a host name site a site, but don't need to declare a new binding (because you're inheriting a binding from the global level) then you can use the new `hostAlias` key at the site level which can be a list or array of hostnames to be layered on top of the global bindings for this site. &#x20;

{% code title=".site.json" %}
```json
{
    "profile":"production",
    "directoryBrowsing":false,
    "hostAlias":"example.com,www.example.com",
    "GZipEnable":false
}
```
{% endcode %}

In Multisite mode, each site can have as many bindings as you want, were each binding has

* An IP address (or all IPs, which is the equivalent to `0.0.0.0`)
* A port
* Zero or more hostnames
  * Exact (full) match like `www.foo.com`
  * Starts-with match like `*.foo.com` or `*bar.com`
  * Ends-with match like `www.foo.*` or `www.bar*`
  * Regular expression match like `~www\.(bob|tom|bernard)Industries\.(com|net|org)`

When a request comes into CommandBox in multi-site mode, the most specific binding will be found, and the traffic served to the matching site.  The order of precedence of binding matching is as follows:

* Exact IP and hostname match
* Exact IP and hostname ends with match
* Exact IP and hostname starts with match
* Exact IP and hostname regex match
* Any IP and hostname exact match
* Any IP and hostname ends with match
* Any IP and hostname starts with match
* Any IP and hostname regex match
* Try Exact IP and any hostname
* Any IP and any hostname
* Default site

If you need help debugging how your bindings are being assembled at runtime, you can use this command:

```
server info --verbose
```

which will show you all the listeners (ports/IPs) that CommandBox is listening to at the OS level as well as all the bindings for each site that control what listeners will map to them. &#x20;

```

  - site1: http://site1.com:80 --> C:\path\to\site1\
      Bindings:
        - 0.0.0.0:80:site1.com

  - site2: http://site2.com:80 --> C:\path\to\site2\
      Bindings:
        - 0.0.0.0:80:site2.com

  - site3: http://site3.com:80 --> C:\path\to\site3\
      Bindings:
        - 0.0.0.0:80:site3.com

  - default: http://127.0.0.1:80 --> C:\path\to\default\
      Bindings:
        - 0.0.0.0:80:*
        - Default Site

  Listeners:
    - HTTP
      - 0.0.0.0:80

```

If you do not specify any bindings at all, it will still pick a random IP address to bind to.  It will also pick a SEPARATE random port for each site so you can access each site separately. &#x20;

Each site can have a default open browser URL, but CommandBox will not automatically open a browser in multi-site mode.  These URLs will be available in a sub menu of the tray icon, or can be used from the `server open` command like so:

```
server open siteName=site3
```

### Default site

If no bindings were matched, then CommandBox will look for a default site configured.  Unlike IIS which has a default site created by scratch, or Apache which just takes the first site defined, CommandBox requires you to explicitly define a default site like NGinx.  If no default site is defined, CommandBox has a built in "site not found" error page that will be shown.  You can avoid or "turn off" this default page by setting a default site.&#x20;

Configure any of your sites to be the default site by adding a `default` key set to `true` inside the site JSON.  It doen't matter whether the site is defined in the `server.json` in the `sites` object or in an external site JSON file.

```
{
  "webroot" : "www/default",
  "default" : true
}
```

