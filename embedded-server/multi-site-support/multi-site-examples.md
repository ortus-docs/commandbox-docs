# Multi-Site Examples

This Github repository has several Multi-Site examples in the folders starting with the words `multi-site-`.  Here is an overview of a few of them.

### Basic Multi-Site configured in server.json

Here is a basic `server.json` configuring several sites.  Here we have some defaults configured in the `web` object for all sites, including a single HTTP binding to all IPs on port 80.  There is a custom host alias configured on each site.  There is a site called "default" which is set t the default site with `default=true` in the site object.

{% code title="server.json" %}
```json
{
  "name": "commandbox-test-multi-site-basic",
  "web": {
    "accessLogEnable": "true",
    "aliases": {
      "/js": "javascript"
    },
    "allowedExt": "log",
    "errorPages": {
      "404": "404.cfm"
    },
    "GZIPEnable": "true",
    "gzipPredicate": "regex('^.*\\.txt$')",
    "mimeTypes": {
      "log": "text/plain"
    },
    "rules": [
      "path(/tea)->set-error(418)"
    ],
    "rulesFile": ".rules.txt",
    "welcomeFiles": "custom.cfm,index.cfm",
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
      "webroot": "site2",
      "accessLogEnable": "false",
      "aliases": {
        "/js": "site2/javascript"
      },
      "allowedExt": "log2",
      "GZIPEnable": "false",
      "mimeTypes": {
        "log2": "application/xml"
      },
      "rules": [
        "path(/brad)->set-error(123)"
      ],
      "rulesFile": "site2/.rules.txt",
      "welcomeFiles": "index.cfm",
      "directoryBrowsing": true,
      "blockCFAdmin": true
    },
    "site3": {
      "hostAlias": "site3.com",
      "webroot": "site3",
      "aliases": {
        "/js-brad": "site3/javascript"
      },
      "errorPages": {
        "404": "missing.cfm"
      },
      "welcomeFiles": "main.cfm,default.cfm,index.cfm",
      "directoryBrowsing": false,
      "blockCFAdmin": false
    },
    "default": {
      "default": true,
      "webroot": "default",
      "rules": [
        "rewrite(/index.cfm)"
      ]
    }
  }
}
```
{% endcode %}

Full example here: [https://github.com/Ortus-Solutions/commandbox-tests/tree/master/multi-site-basic](https://github.com/Ortus-Solutions/commandbox-tests/tree/master/multi-site-basic)

### Custom Bindings

This example server contains custom HTTP and SSL bindings for each server.  You'll probably never need anything this complex, but we support it all!  The HTTP port 81 binding on all IPs, SSL binding on port 444, and AJP binding on port 8010 are shared by all sites.  Each site then adds additional bindings that apply only them.  Needless to say, there are multiple URLs that lead to each site.&#x20;

{% code title="server.json" %}
```javascript
{
    "name":"commandbox-tests-multi-site-bindings",    
    "web":{
        "host":"0.0.0.0",
        "HTTP":{
            "port":"80"
        },
        "SSL":{
            "enable":"true",
            "port":"443",
            "certFile":"../certs/ServerCert-all-SAN.pfx"
        },
        "AJP":{
            "enable":"true",
            "port":"8009",
            "secret":"8009secret"
        },
        "bindings":{
            "HTTP":{
                "listen":"0.0.0.0:8080"
            },
            "SSL":{
                "listen":"0.0.0.0:8443",
                "certFile":"../certs/ServerCert-all-SAN.pfx"
            },
            "AJP":{
                "listen":"0.0.0.0:16009",
                "secret":"16009secret"
            }
        }
    },
    "sites":{
        "site1":{
            "hostAlias":"site1.com",
            "webroot":"site1",
            "bindings":{
                "HTTP":{
                    "listen":"81"
                },
                "SSL":{
                    "listen":"444",
                    "certFile":"../certs/ServerCert-all-SAN.pfx"
                },
                "AJP":{
                    "listen":"8010",
                    "secret":"8010secret"
                }
            }
        },
        "site2":{
            "hostAlias":"site2.com",
            "webroot":"site2",
            "bindings":{
                "http":[
                    {
                        "listen":"0.0.0.0:80"
                    },
                    {
                        "listen":"0.0.0.0:82"
                    }
                ],
                "ssl":{
                    "listen":"0.0.0.0:446",
                    "certFile":"../certs/ServerCert-all-SAN.pfx"
                },
                "AJP":{
                    "listen":"0.0.0.0:8011",
                    "secret":"8011secret"
                }
            }
        },
        "site3":{
            "webroot":"site3"
        }
    },
    "hostAlias":"site3.com"
}
```
{% endcode %}

Full example here: [https://github.com/Ortus-Solutions/commandbox-tests/tree/master/multi-site-bindings](https://github.com/Ortus-Solutions/commandbox-tests/tree/master/multi-site-bindings)

### Web root convention

This example server configures some defaults in the `web` object and then simply points to the web root of several sites where a `.site.json` file is waiting to further configure each site.

{% code title="server.json" %}
```javascript
{
    "name":"commandbox-test-multi-site-json-webroot-convention",
    "web":{
        "mimeTypes":{
            "log":"text/plain"
        },
        "rules":[
            "path(/tea)->set-error(418)"
        ],
        "welcomeFiles":"custom.cfm,index.cfm",
        "bindings":{
            "HTTP":{
                "listen":"80"
            }
        }
    },
    "sites":{
        "site1":{
            "webroot":"site1"
        },
        "site2":{
            "webroot":"site2"
        },
        "site3":{
            "webroot":"site3"
        },
        "default":{
            "webroot":"default"
        }
    }
}
```
{% endcode %}

And here are the `.site.json` files from each of the web roots.

{% code title="site1/.site.json" %}
```javascript
{
    "hostAlias":"site1.com"
}
```
{% endcode %}

{% code title="site2/.site.json" %}
```javascript
{
    "hostAlias":"site2.com",
    "accessLogEnable":"false",
    "aliases":{
        "/js":"javascript"
    },
    "allowedExt":"log2",
    "GZIPEnable":"false",
    "mimeTypes":{
        "log2":"application/xml"
    },
    "rules":[
        "path(/brad)->set-error(123)"
    ],
    "rulesFile":"site2/.rules.txt",
    "welcomeFiles":"index.cfm",
    "directoryBrowsing":true,
    "blockCFAdmin":true
}
```
{% endcode %}

{% code title="site3/.site.json" %}
```javascript
{
    "hostAlias":"site3.com",
    "aliases":{
        "/js-brad":"javascript"
    },
    "errorPages":{
        "404":"missing.cfm"
    },
    "welcomeFiles":"main.cfm,default.cfm,index.cfm",
    "directoryBrowsing":false,
    "blockCFAdmin":false
}
```
{% endcode %}

Full example here: [https://github.com/Ortus-Solutions/commandbox-tests/tree/master/multi-site-json-webroot-convention](https://github.com/Ortus-Solutions/commandbox-tests/tree/master/multi-site-json-webroot-convention)

### Per-site siteConfigFile

This example is similar to above, but instead of pointing to the web root of each site, it points to the site config file for each site, which are stored outside of the web roots.&#x20;

{% code title="server.json" %}
```javascript
{
    "name":"commandbox-test-multi-site-siteConfigFile",
    "web":{
        "bindings":{
            "HTTP":{
                "listen":"80"
            }
        }
    },
    "sites":{
        "site1":{
            "siteConfigFile":"site1.json"
        },
        "site2":{
            "siteConfigFile":"site2.json"
        },
        "site3":{
            "siteConfigFile":"site3.json"
        },
        "default":{
            "siteConfigFile":"default.json"
        }
    }
}
```
{% endcode %}

In this example, the `site1.json`, `site2.json`, `site3.json`, and `default.json` files all live in the same directory as the server.json and would point to their respective webroots like so:

{% code title="site1.json" %}
```javascript
{
    "hostAlias":"site1.com",
    "webroot":"site1"
}
```
{% endcode %}

Full example here: [https://github.com/Ortus-Solutions/commandbox-tests/tree/master/multi-site-siteConfigFile](https://github.com/Ortus-Solutions/commandbox-tests/tree/master/multi-site-siteConfigFile)

### SiteConfigFiles globbing pattern

In this example, we have a single file globbing pattern that points to a directory of JSON files that define the sites:

{% code title="server.json" %}
```javascript
{
    "name":"commandbox-test-multi-site-siteConfigFiles-globbing",
    "web":{
        "bindings":{
            "HTTP":{
                "listen":"80"
            }
        }
    },
    "siteConfigFiles":"sites-enabled/*.json"
}
```
{% endcode %}

In this example, we have a folder called `sites-enabled` in the same directory as the `server.json` with one or more JSON files.  Here is an example of one.  Note the relative paths are always relative to this file.

{% code title="sites-enabled/site1.json" %}
```javascript
{
    "hostAlias":"site1.com",
    "webroot":"../site1"
}
```
{% endcode %}

Full example here: [https://github.com/Ortus-Solutions/commandbox-tests/tree/master/multi-site-siteConfigFiles-globbing](https://github.com/Ortus-Solutions/commandbox-tests/tree/master/multi-site-siteConfigFiles-globbing)
