# Bindings

HTTP bindings are much more robust in multi-site mode.  In single-site mode, there is basically no such thing as host name bindings.  All traffic coming into that CommandBox server is simply routed to the one and only server regardless of its host name or IP address. &#x20;

As of version 6.0, a command server can now have

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

Configure any of your sites to be the default site by adding a `default` key set to `true` inside the site JSON.  It doesn't matter whether the site is defined in the `server.json` in the `sites` object or in an external site JSON file.

```
{
  "webroot" : "www/default",
  "default" : true
}
```
