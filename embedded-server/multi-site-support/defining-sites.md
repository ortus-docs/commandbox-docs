---
description: Defining sites in CommandBox Multi-Site
---

# Defining Sites

There's no flag or setting for enabling multiple sites in a single server.  All you have to do is

* define a `sites` object in `server.json`
* or a `siteConfigFiles` setting in `server.json`

inside your `server.json` and if one or more sites are found, CommandBox will automatically switch over to "multi-site" mode.   It's easy to tell if a server has more than one site.  When you start it, the console output will have a dedicated section in the output for each site.  There is also additional information in the `server info` command about each site.

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

Let's explore the two methods of defining more sites.

### Define a "sites" object in server.json

The `sites` object will have a key for each site you want to define where the name of the key is the name of each site and the value is another object containing the configuration of that site.  Think of all of the setting which can be set in the `web` object of `server.json`.  Each site object in the `sites` object can have all the same settings as the top-level `web` object so it will be very familiar.

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

* `webroot`
* or `siteConfigFile`  which points to a JSON file on disk where a web root must be defined

{% code title="server.json" %}
```json
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
{% endcode %}

If you want to access each site individually, you'll either want to specify a separate HTTP/SSL/AJP binding OR a custom hostname for each site.  Otherwise, you'll have no way to access them!

You can also place a `.site.json` file inside of the web root, which will be found by convention and its contents will be added to the site object for that site.  This is akin to configuring your sites in Apache, but having an `.htaccess` file in each web root to override settings.

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

System setting expansions will be processed in any of the site JSON files located via your file globbing patterns.  If the `server.json` has a `sites` object AND a `siteConfigFiles` setting, all sites will be combined together.  Feel free to choose the approach that fits your needs!
