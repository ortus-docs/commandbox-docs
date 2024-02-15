# What's New in 6.0.0

There are a lot of new features in CommandBox 6.  Here's an overview of the biggest ones.  Check out the release notes for the full list.

### Multi-Site Servers

This one is huge.  It's the hallmark feature of CommandBox 6 and allows you to easily run as many web sites (with different web roots) in a single CommandBox server.  This finally gives you the same behavior you get with Adobe ColdFusion and IIS or Lucee/Tomcat and Apache with mod\_cfml.  CommandBox has had built-in ModCFML support for a while, but it still needed a web server in front to work fully.  CommandBox Multi-Site gives you a fully powered web server that allows you to define as many separate web sites as you like, each with COMPLETE configuration control, all inside a single server process. &#x20;

* Rewrites
* web aliases
* security profile&#x20;
* HTTP/SSL/AJP bindings
* SSL Certs
* welcome files
* MIME types
* GZIP settings
* Basically everything can currently configure under the "web" object of your server.json can be set on a per-site basis!

CommandBox is now truly a One-Stop-Shop for running your apps from development to production.  You don't need Apache, Nginx, IIS, or Tomcat!

```
{
  "name": "commandbox-multi-site",
  "web": {
    "bindings": {
      "HTTP": {
        "listen": "80"
      }
    }
  },
  "sites": {
    "site1": {
      "hostAlias": "site1.com",
      "webroot": "site1"
    },
    "site2": {
      "hostAlias": "site2.com",
      "webroot": "site2"
    },
    "site3": {
      "hostAlias": "site3.com",
      "webroot": "site3"
    }
  }
}
```

Read more here: [https://commandbox.ortusbooks.com/embedded-server/multi-site-support](https://commandbox.ortusbooks.com/embedded-server/multi-site-support)

### Enhanced Server Bindings

Going hand-in-hand with our Multi-Site features, is the ability to bind to more than one HTTP port, more than one SSL port and more than one AJP port.  You can configure proper host name matching on any binding, and even have multiple SSL certs.  This new feature is available not only for Multi-Site but also for single site servers.  The new server bindings come with a new JSON syntax in the server.json (we still support the old one too)

```
{
    "web" : {
        "bindings" : {
            "HTTP" : {
                "listen" : "10.10.0.123:8080",
                "host" : "site.com,site2.net"
            }
        }
    }
}
```

Read more here: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/bindings](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/bindings)

### SSL SNI Support

Going hand-in-hand with our enhanced server bindings, is the ability not only to be able to specify multiple SSL certs per site, but also multiple SSL certs per SSL binding.  CommandBox automatically enables SNI (Server Name Indication) which will choose the proper cert based on the incoming host name.  With support for PEM files, DER formats, and PFX formats, this really opens up a lot of capabilities.

Read more here: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/bindings#ssl-sni-support](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/bindings#ssl-sni-support)

### Rewrite Maps

We added a popular feature from Apache's mod\_rewrite called rewrite maps.  This allows you to create a simple text file of values you can reference in your rewrite rules to map incoming URL values to another value.

```
{
  "web" : {
    "rules":[
      "rewrite-map( name=myMap, file='/path/to/myMap.txt' case-sensitive=false )",
      "regex-nocase( '^/foo/(.*)$' ) -> rewrite( 'index.cfm?page=%{map:myMap:$[1]|99}' )"
    ]
  }
}
```

Read more here: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/server-rules/rewrites-map](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/server-rules/rewrites-map)

### Publish command directly uploads to S3

This is a nice little productivity enhancement.  When you run the publish command, the CLI will now directly upload your zip file to S3 instead of sending it to ForgeBox first.  This improves the speed and efficiency of your deployments.

### Add a **proxy** server rule alias

This is a simple one.  If you want to create a simple reverse proxy to a single back-end server, we've created an alias for the existing load-balanced-proxy() handler called just proxy which accepts a single host with less verbosity.

```
proxy( 'http://localhost:8085' )
```

Read more here: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/server-rules/rule-examples](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/server-rules/rule-examples)

### Specify Package and Server scripts as an array

You're already familiar with specifying package scripts and server scripts in your **box.josn** and **server.json** as a string containing one or more commands.  Instead of using **&&** for multiple commands, you can also do this by specifying an array of strings instead of a string like so:

```
{
  "name" : "My Package",
  "scripts" : {
      "build" : [
          "!grunt build",
          "testbox run",
          "run-script generateAPIDocs",
          "bump --patch && publish"
       ],
  }
}
```

\
This can be much more readable for multiple commands.  Note, this is functionality equivalent to using **&&**, which means any erroring command will stop execution.

Read more here: [https://commandbox.ortusbooks.com/package-management/package-scripts#running-multiple-commands](https://commandbox.ortusbooks.com/package-management/package-scripts#running-multiple-commands)

### ColdBox, TestBox and ContentBox commands are now modules

CommandBox still bundles helpful scaffolding commands for your favorite MVC framework Testing framework, and CMS, but these commands are no longer part of the core CommandBox source code.  We've given them new life as independent modules.  They are installed by default, but they now have their own lifecycle and can get releases at any time.  You can view and update them with the rest of your system modules.

```
list --system
update --system
```

Read more here: [https://commandbox.ortusbooks.com/package-management/system-modules](https://commandbox.ortusbooks.com/package-management/system-modules)

### Adobe CF Script Alias

CommandBox adds a **/cf\_scripts/scripts** alias for you any time you start an Adobe CF server.  This alias points to the same folders in the root of the Adobe WAR.  If you set a custom scripts src path in the CF administrator then you'll want to ensure CommandBox uses the expected alias.  There is now a setting called **web.adobeScriptsAlias** which allows you to control the public, web-accessible path to the scripts folder that CommandBox creates for you.  And better yet, if you're using CFConfig, will automatically update the Adobe script source setting to match and vice versa.

Read more here: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/adobe-cfpm#script-alias](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/adobe-cfpm#script-alias)

### CommandBox Pro Config settings auto-sync

As a perk of CommandBox pro, once you log into the CLI with your ForgeBox Pro account, your config settings will now **automatically** sync to and from ForgeBox.  This is a great way to keep multiple CLI instances across computers up to date. &#x20;

Read more here: [https://commandbox.ortusbooks.com/config-settings/setting-sync#automatic-sync-of-settings](https://commandbox.ortusbooks.com/config-settings/setting-sync#automatic-sync-of-settings)

### Release Notes

Here are the full release notes for CommandBox 6.0.0

#### Bug

[COMMANDBOX-1590](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1590)Some server.json config options unavailable as environment variables

[COMMANDBOX-1598](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1598)Runwar doesn't load servlet filter mappings correct in web.xml override

[COMMANDBOX-1604](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1604) semantic version parsing ignores part of pre-release IDs with hyphen

[COMMANDBOX-1608](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1608) Stackoverflow when using serverinfo system setting expansion in a server script

[COMMANDBOX-1613](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1613) Lucee Light Engine

[COMMANDBOX-1615](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1615) Installing package crashes when PackageService cannot delete tmp folder

#### New Feature

[COMMANDBOX-1589](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1589) Add a \`proxy\` server rule alias to \`load-balanced-proxy\` which takes in one item instead of an array.

[COMMANDBOX-1592](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1592) Add Rewrite Map feature similar to Apache

[COMMANDBOX-1593](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1593) Multi-Site mode

[COMMANDBOX-1611](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1611) CommandBox Pro users get config auto sync

#### Improvement

[COMMANDBOX-1477](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1477) box install \<package>: constrain to version already defined in box.json

[COMMANDBOX-1572](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1572) Publish command directly upload to S3

[COMMANDBOX-1591](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1591) Allow the scripts key under the server.json and box.json to use an array of scripts to run under the same script key name

[COMMANDBOX-1594](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1594) XML formatting in print helper can kick in a bad time

[COMMANDBOX-1595](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1595) Allow control over undertow' s transferMinSize

[COMMANDBOX-1599](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1599)Provide way to escape literal colon (:) command parameter name

[COMMANDBOX-1600](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1600)Remove contentbox, coldbox, etc modules from the core

[COMMANDBOX-1603](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1603)Customize Adobe cf scripts alias

#### Task

[COMMANDBOX-1609](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1609)Update bundled JRE to 11.0.22+7

[COMMANDBOX-1610](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1610)Update to Lucee 5.4.4.38
