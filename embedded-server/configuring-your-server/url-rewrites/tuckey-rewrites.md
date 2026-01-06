# Tuckey Rewrites

{% hint style="warning" %}
Note: Tuckey-based URL rewrites are not recommended going forward.  They are limited and do not play with with [Multi-Site](../../multi-site-support/) servers.  It is recommended you move to the more-powerful [Server Rules](../server-rules/).
{% endhint %}

In addition to the recommended [Server Rules](../server-rules/), CommandBox still supported the deprecated [Tuckey servlet filter](http://tuckey.org/urlrewrite/) library.  It's not recommended to use going forward, but these docs are a reference for those still using it.

Tuckey rewrites can't be used with Multi-site due to limitations of how the are loaded as a servlet filter, which may be shared across multiple sites.

## Custom Rules

If you want to customize your rewrite rules, just create your own XML file and specify it when starting the server with the `rewritesConfig` parameter. Here we have a simple rewrite rule that redirects `/foo` to `/index.cfm`&#x20;

{% hint style="info" %}
The basic framework rewrite rule can be found in `~\.CommandBox\cfml\system\config\urlrewrite.xml`
{% endhint %}

**customRewrites.xml**

```markup
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE urlrewrite PUBLIC "-//tuckey.org//DTD UrlRewrite 4.0//EN" "http://tuckey.org/res/dtds/urlrewrite4.0.dtd">
<urlrewrite>
    <!-- this will redirect the user from /foo to /index.cfm -->
    <rule>
        <from>^/foo$</from>
        <to type="redirect">/index.cfm</to>
    </rule>
    <!-- internally redirect the requested URL from /gallery to /index.cfm?page=gallery with query string appended -->
    <rule>
        <from>^/gallery</from>
        <to type="passthrough" qsappend="true">/index.cfm?page=gallery</to>
    </rule>

</urlrewrite>
```

Then, fire up your server with its custom rewrite rules:

```bash
start --rewritesEnable rewritesConfig=customRewrites.xml
```

In `server.json`

```bash
server set web.rewrites.enable=true
server set web.rewrites.config=customRewrites.xml


server show web.rewrites.enable
server show web.rewrites.config
```

You can place your custom rewrite rule wherever you like, and refer to it by using either a relative path or an absolute path. CommandBox will start looking relative to where the `server.json` file resides.

```bash
server set web.rewrites.enable=true
server set web.rewrites.config=/my/custom/path/customRewrites.xml
```

or

```bash
server set web.rewrites.enable=true
server set web.rewrites.config=~\.CommandBox\cfml\system\config\customRewrites.xml
```

## Apache mod\_rewrite-style rules

If you're coming from Apache, Tuckey supports a large subset of the `mod_rewrite` style rules like what you would put in `.htaccess`. You can simply put your rules in a file named `.htaccess` and point the `web.rewrites.config` property to that file.

_Note: The name of the file matters with mod\_rewrite-style rules. It must be called_ `.htaccess`_. With xml rewrites, the filename is not important, only the content._

Here are some simple rewrite rules:

```bash
RewriteEngine on

#The ColdBox index.cfm/{path_info} rules.
RewriteRule ^$ index.cfm [QSA,NS]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ /index.cfm/%{REQUEST_URI} [QSA,L,NS]

RewriteRule ^/foo/                         /

# Defend your computer from some worm attacks
RewriteRule .*(?:global.asa|default\.ida|root\.exe|\.\.).* . [F,I,O]

# Redirect Robots to a cfm version of your robots.txt
RewriteRule ^/robots\.txt                   /robots.cfm

# Change your default cfm file to index.cfm
RewriteRule ^/default.cfm                   /index.cfm [I,RP,L]
RewriteRule ^/default.cfm((\?.+)|())$       /index.cfm$1  [I,RP,L]

RewriteRule ^/News.html((\?.+)|())$         /News/index.cfm$1 [I,RP,L]

# redirect mozilla to another area
RewriteCond  %{HTTP_USER_AGENT}  ^Mozilla.*
RewriteRule  ^/no-moz-here$                 /homepage.max.html  [L]
```

Please see the docs here on what's supported:

[http://cdn.rawgit.com/paultuckey/urlrewritefilter/master/src/doc/manual/4.0/index.html#mod\_rewrite\_conf](http://cdn.rawgit.com/paultuckey/urlrewritefilter/master/src/doc/manual/4.0/index.html#mod_rewrite_conf)

> **info** For more information on custom rewrite rules, consult the [Tuckey docs](http://cdn.rawgit.com/paultuckey/urlrewritefilter/master/src/doc/manual/4.0/index.html#configuration).

## SES URLs

Your servers come ready to accept SES-style URLs where any text after the file name will show up in the `cgi.path_info`. If rewrites are enabled, the `index.cfm` can be omitted.

```
site.com/index.cfm/home/login
```

SES URLs will also work in a sub directory, which used to only work on a "standard" Adobe CF Tomcat install. Please note, in order to hide the `index.cfm` in a subfolder, you'll need a custom rewrite rule.

```
site.com/myFolder/index.cfm/home/login
```

## Logging

The Tuckey Rewrite engine has debug and trace level logging that can help you troubleshoot why your rewrite rules aren't (or _are_) firing. To view these logs, simply start your server with the `--debug` or `--trace` flags. Trace shows more details than debug. These options work best when starting in `--console` mode so you can watch the server logs as you hit the site. Alternatively, you can follow the server's logs with the `server log --follow` command.

```
start --debug
server log --follow
```

## Additional Tuckey Settings

The Tuckey Rewrite library that CommandBox uses under the hood. It has some extra settings that CommandBox allows you to use.

### Watch rewrite file for changes

To monitor your custom rewrite file for changes without needing to restart the server, use this setting.

```
server set web.rewrites.configReloadSeconds=30
```

### Internal Tuckey status page

To enable the inbuilt Tuckey status page, use the following setting. Note, `debug` mode needs to be turned on for the Tuckey status page to work. Also, you'll need to customize your rewrite file if you use a path other than `/tuckey-status`.

```
server set web.rewrites.statusPath=/tuckey-status
```
