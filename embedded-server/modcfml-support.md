# ModCFML Support

Traditional CommandBox servers have only a single web root and if you want to start up two separate sites, then you start two CommandBox servers which creates two separate Java processes. This may be cumbersome or even untenable if you have hundreds site separate sites you want to run as the overhead of the JVMs would require too much memory.

CommandBox can also use a popular standard called ModCFML which allows it to behave like an Adobe ColdFusion Tomcat-based installation, which you can set behind a web server and CF will pull the webroots dynamically from the web server, allowing any number of separate sites to all be run on a single JVM instance.

### What is ModCFML?

ModCFML takes it's name from the convention of Apache modules, which are named `mod_xyz` and is really more of a standard implemented by several different libraries. The way ModCFML works, is the front-end web server (IIS, Apache, Nginx, etc) passes a few custom HTTP headers to the back end CF server on each request that tells the CF server where the web root lives for that site. Here's an overview of the related libraries:

* **BonCode** - Boncode is an AJP proxy for IIS which can be configured globally or on a per-site basis to proxy requests to a back-end CF server. Boncode automatically sends the ModCFML HTTP headers. [Docs here.](http://www.boncode.net/boncode-connector)
* **Apache mod\_cfml module** - This module can be installed into Apache HTTPd server to create an AJP reverse proxy. [Docs here.](https://github.com/viviotech/mod_cfml)
* **Tomcat ModCFML Valve** - This Tomcat valve creates contexts in Tomcat for the incoming ModCFML Headers. Since you're using CommandBox, you don't need this since CommandBox has support for the equivalent behavior built in!
* **CommandBox** - CommandBox will obey the standard ModCFML HTTP headers so long as `ModCFML.enable` is set to `true` for the server.
* **Nginx** or other web servers - Even though there is no native ModCFML module for Nginx, you can still use it by just setting the necessary HTTP headers manually! More on this below.

### Set Up

This is the minimum needed to configure ModCFML for any front-end web server in CommandBox:

```bash
server set ModCFML.enable=true
server set ModCFML.sharedKey=my-secret
server set web.ajp.enable=true web.ajp.port=8009
```

That's it! The default web root for the server will be whatever folder you start the CommandBox sever in, but that default web root will only be used if a requests reaches CommandBox which doesn't contain the special ModCFML HTTP headers.

Next you'll need to install BonCode (if using IIS) or mod\_cfml (if using Apache) and configure it with the same shared key for security. The web server doesn't need to be on the same server as CommandBox, but CommandBox's AJP port does need to be accessible to the web server and the port numbers need to match. 8009 is the default AJP port, but it's not a requirement.

### Configuration

Here are some additional ModCFML settings you may want to tweak:

#### maxContexts

This limits the number of contexts which can be created to prevent a malicious client from causing a DOS from a huge number of contexts, which would eventually begin consuming memory. Default is `200`.

#### requireSharedKey

Set this to `false` FOR DEVELOPMENT PURPOSES ONLY to not require the shared key header to be present. This may simplify your local setup, but do NOT enable this on any externally-accessible web server as a malicious client could send their own HTTP headers and serve content from anywhere on your server's hard drive! Default is `true`.

### **Unsupported Configuration**

Here are some settings you may be familiar with in the Tomcat valve, that CommandBox does NOT use.

#### **timeBetweenContexts**

CommandBox does NOT have a corresponding setting for `timeBetweenContexts` \*\*\*\* since Undertow is VERY fast to add new contexts.

#### **scanClassPaths**

CommandBox does NOT have a corresponding setting for `scanClassPaths` \*\*\*\* because it re-uses the existing class loader and cloned Undertow deployment from the default context so no jars at all need to be scanned.

#### **loggingEnabled**

CommandBox does NOT have a corresponding setting for `loggingEnabled`. There is already logging as part of Runwar and the `--debug` flag when you start your server will automatically log additional bits.

#### **responseCode**

CommandBox does NOT have a corresponding setting for `responseCode` \*\*\*\* because it is capable of adding the new context entirely on the fly and using it immediately without needing to re-send the request.

### What about Nginx, etc?

You can use any front end web server you like so long as you manually set the HTTP headers that ModCFML requires. Here they are:

* `X-ModCFML-SharedKey` - Needs to match the `ModCFML.sharedKey` in your `server.json`
* `X-Tomcat-DocRoot` - Needs to contain the absolute path to the web root for the current virtual host
* `X-Webserver-Context` - Optional. Will be used instead of hostname to group multiple hostnames into a single context
* `X-VDirs` - A string representing the aliases (virtual directories) of the web server for CommandBox to auto-create in the servlet in the format: `/foo,C:\path\to\foo;/bar,C:\path\to\bar`
* `X-VDirs-SharedKey` - Must also contain the same value as `X-ModCFML-SharedKey` or `X-VDirs` will be ignored. This is for security since the older mod\_cfml Apache proxies don't override a missing `X-VDir` header, allowing malicious client to pass one. Only pass the `X-VDirs-SharedKey` header if your proxy is explicitly controlling the contents of the `X-VDirs` header.

For NGinx, setting the headers would look like this and would need configured for each site.

```
proxy_set_header X-Tomcat-DocRoot /path/to/site/www;
proxy_set_header X-ModCFML-SharedKey SHARED-KEY-HERE;
```

It is not required to use an AJP proxy. CommandBox's ModCFML support will process the incoming HTTP headers regardless of what protocol was used to proxy the requests to it.

### Debugging

How do you confirm ModCFML is working or debug when it doesn't go right? CommandBox has automatic `debug` and `trace` level messages that will appear in the console when you start your server with the `--debug` or `--trace` flag that will tell you when a new context is deployed and what the web root is.

```
[WARN ] Runwar: X-Tomcat-DocRoot is null or empty.  Using default context for deploymentKey [127.0.0.1].
[INFO ] Runwar: Creating deployment [site1.com] in C:\www\site1
[DEBUG] Runwar: New servlet context created for [site1.com]
[INFO ] Runwar: Creating deployment [site2.com] in C:\www\site2
[DEBUG] Runwar: New servlet context created for [site2.com]
[INFO ] Runwar: Creating deployment [site3.com] in C:\www\site3
[DEBUG] Runwar: New servlet context created for [site3.com]
```

### Adobe vs Lucee

CommandBox's ModCFML support will work for both Adobe and Lucee servers! Keep in mind however, that Lucee and Adobe work differently under the covers, even though it looks the same from the outside. When the server is Lucee, CommandBox's ModCFML support will create a new servlet context deployment for each web context. Each servlet context will have a context root of `/` and will share the same class loader as the default instance, so they will also share the same server context. Adobe only uses the default servlet context, but swaps out the resource manager per-request, just like Adobe's Tomcat-based installations work. If you don't know what all that means, don't worry.

### Naked ModCFML

You maybe wondering if it's possible to make use of multiple web roots in CommandBox without having any web server at all. Well, we weren't going to let you in on this, but actually YES it is possible! All you need to do is configure some Server Rules to detect the host name and set the HTTP headers and CommandBox's ModCFML will be none the wiser. You can turn off the requirement of a shared key, but make sure you have a catch-all rule that rejects requests from unknown host names.

Here is the `server.json` for a simple setup. Note, we're using the `commandbox-hostupdater` module here to auto-create `hosts` file mappings for our domains. We're also pointing the webroots to `site1.com`, `site2.com`, and `site3.com` folders inside of the default web root, but these could really point anywhere you like.

```javascript
{
    "ModCFML":{
        "enable":"true",
        "requireSharedKey":"false"
    },
    "web":{
        "hostAlias":[
            "site1.com",
            "site2.com",
            "site3.com",
            "fakedomain.com"
        ],
        "rules":[
            "equals( %{LOCAL_SERVER_NAME}, 'site1.com' ) -> set(attribute=%{i,X-Tomcat-DocRoot},value='${serverinfo.webroot}site1.com')",
            "equals( %{LOCAL_SERVER_NAME}, 'site2.com' ) -> set(attribute=%{i,X-Tomcat-DocRoot},value='${serverinfo.webroot}site2.com')",
            "equals( %{LOCAL_SERVER_NAME}, 'site3.com' ) -> set(attribute=%{i,X-Tomcat-DocRoot},value='${serverinfo.webroot}site3.com')",
            "not regex( value=%{LOCAL_SERVER_NAME}, pattern='^site[123]\\.com$', case-sensitive=false ) -> set-error(401)"
        ]
    }
}
```

If you want something SUPER quick and dirty for local development and you TRUST the people accessing the site, you could get really clever and just have a simple convention where the host name matches a a folder inside of your default web root. Any hostnames that didn't match a real folder would just use the default web root.

```json
{
    "ModCFML":{
        "enable":"true",
        "requireSharedKey":"false"
    },
    "web":{
        "rules":[
            "set(attribute=%{i,X-Tomcat-DocRoot},value='${serverinfo.webroot}%{LOCAL_SERVER_NAME}')"
        ]
    }
}
```
