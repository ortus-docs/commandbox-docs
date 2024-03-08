# Bindings

You can configure the IP, Port, and hostnames for your servers in the `bindings` object, which is new in CommandBox 6.0.  Whereas the [legacy syntax](server-port-and-host.md) defaults to binding to `localhost`, bindings will default to all IPs or `0.0.0.0` which is more consistent without mainstream web servers work.

The `bindings` object goes inside your `web` object in `server.json` and for [Multi-Site](../../multi-site-support/) servers, you can also specify `bindings` in each site as well.

### Types of Bindings

There are 3 types of bindings, some of which have additional information that is specific to them

* **HTTP**
* **SSL**
  * HTTP/2 support
  * Server certs
  * Client certs
* **AJP**
  * AJP secret

Each type of binding is represented by an object of that name inside the `bindings` object.  Everything is optional, so only specify what you need

{% code title="server.json" %}
```json
{
    "web" : {
        "bindings" : {
            // A single binding
            "HTTP" : {},
            // or an array of objects for multiple bindings
            "HTTP" : [],
            
            // A single binding
            "SSL" : {},
            // or an array of objects for multiple bindings
            "SSL" : [],
            
            // A single binding
            "AJP" : {},
            // or an array of objects for multiple bindings
            "AJP" : []
        }
    }
}
```
{% endcode %}

### Creating a binding

Every binding has

* IP address (can be `*` or `0.0.0.0` which means All IPs)
* Port
* zero or more hostnames (An empty string or `*` will match all hostnames)

Note, hostnames are only really used for Multi-Site servers.  If you only have a single site defined, all traffic will be served by that site regardless of the hostname of the incoming request.

#### Just the port

The default key to use is called `listen`.  You can specify JUST a port, which will default to all IPs and all hostnames:

```bash
server set web.bindings.HTTP.listen=8080
```

{% code title="server.json" %}
```javascript
{
    "web" : {
        "bindings" : {
            "HTTP" : {
                "listen" : "8080"
            }
        }
    }
}
```
{% endcode %}

#### Listen to IP:port

We can also specify the IP address as an IP or a `*` or `0.0.0.0` before the port delimited by a colon:

```bash
server set web.bindings.HTTP.listen=10.10.0.123:8080
```

{% code title="server.json" %}
```json
{
    "web" : {
        "bindings" : {
            "HTTP" : {
                "listen" : "10.10.0.123:8080"
            }
        }
    }
}
```
{% endcode %}

#### Specify IP and Port separately

As an alternative to the `listen` key, you can specify `IP` and `port` keys.  This can be handy if you plan to override just part of a binding via env vars.

```bash
server set web.bindings.HTTP.IP=10.10.0.123
server set web.bindings.HTTP.port=8080
```

{% code title="server.json" %}
```javascript
{
    "web" : {
        "bindings" : {
            "HTTP" : {
                "IP" : "10.10.0.123",
                "port" : "8080"
            }
        }
    }
}
```
{% endcode %}

This syntax is mutually exclusive with the `listen` key.

#### Host names

Add in as many hostnames as you need as a comma-delimited list or an array

{% code title="server.json" %}
```javascript
{
    "web" : {
        "bindings" : {
            "HTTP" : {
                "listen" : "10.10.0.123:8080",
                "host" : "site.com,site2.net"
                // Or...
                "host" : [
                    "site.com",
                    "site2.net"
                ]
            }
        }
    }
}
```
{% endcode %}

#### Multiple bindings of the same type&#x20;

More than one HTTP binding would look like this, where the same object is used, but inside of an array.

{% code title="server.json" %}
```javascript
{
    "web" : {
        "bindings" : {
            "HTTP" : [
                {
                    "listen" : "10.10.0.123:8080",
                    "host" : "internal-site.com"
                },
                {
                    "listen" : "*:80",
                    "host" : "site.com"
                }
            ]
        }
    }
}
```
{% endcode %}

### AJP Secret

`AJP` bindings work the same as the `HTTP` binding examples above, but can have the addition of a `secret` key.  All AJP connections to this port will need to come bearing the required secret.

```bash
server set web.bindings.ajp.listen=*:8009
server set web.bindings.ajp.secret=my-secret-value
```

{% code title="server.json" %}
```javascript
{
    "web" : {
        "bindings" : {
            "AJP" : {
                "listen" : "*:8009",
                "secret" : "my-secret-value"
            }
        }
    }
}
```
{% endcode %}

### HTTP/2 Support

Technically, HTTP/2 can be enabled on either `HTTP` or `SSL` bindings, but most browsers will only negotiate HTTP/2 over `SSL`.

```bash
server set web.bindings.SSL.HTTP2Enable=true
```

{% code title="server.json" %}
```javascript
{
    "web" : {
        "bindings" : {
            "SSL" : {
                "listen" : "0.0.0.0:443",
                "HTTP2Enable" : true
            }
        }
    }
}
```
{% endcode %}

HTTP/2 is enabled by default.  The legacy `web.http2enable` flag is still obeyed and will be applied to any bindings in that block unless otherwise overridden. &#x20;

### SSL Server Certs

To configure a single SSL Server cert, you can specify the following keys inside the binding:

* `certFile` - A PEM-encoded DER cert or a PFX file
* `keyFile` - THe Private key (not used for PFX)
* `keyPass` - The key pass or PFX pass. Blank if not used

{% code title="server.json" %}
```javascript
{
    "web" : {
        "bindings" : {
            "SSL" : {
                "listen" : "0.0.0.0:443",
                "certFile" : "../certs/mycert.pem",
                "keyFile" : "../certs/mykey.pem",
                "keyPass" : "my-pass"
            }
        }
    }
}
```
{% endcode %}

#### SSL SNI Support

To configure multiple SSL certs on the same binding, use a `certs` array of objects containing the same keys above for each cert you want to specify.&#x20;

{% code title="server.json" %}
```javascript
{
    "web" : {
        "bindings" : {
            "SSL" : {
                "listen" : "0.0.0.0:443",
                "host" : "site1.com,site2.com,site3.com"
                "certs" : [
                    {
                        "certFile" : "../certs/site1Cert.pem",
                        "keyFile" : "../certs/site1Key.pem"
                    },
                    {
                        "certFile" : "../certs/site12Cert.pem",
                        "keyFile" : "../certs/site2Key.pem"
                    },
                    {
                        "certFile" : "../certs/site3Cert.pem",
                        "keyFile" : "../certs/site3Key.pem"
                    }
                ]
            }
        }
    }
}
```
{% endcode %}

CommandBox will automatically use SNI (Server name Indication) to choose the correct cert to use when negotiating the SSL handshake based on the hostnames in each cert's&#x20;

* Subject Common Name (CN)
* SAN (subject alternative names)

CommandBox will also handle SNI for wildcard certs as well.

### SSL Client Certs

If using Client Cert authentication, you can also specify client certs for each SSL binding in an object called `clientCert`.  This object can have the following child keys:

* `mode`
* `CACertFiles`
* `CATrustStoreFile`
* `CATrustStorePass`
* `SSLRenegotiationEnable`

{% code title="server.json" %}
```json
{
  "web" : {
    "bindings" : {
      "ssl" : {
        "listen" : "0.0.0.0:443",
        "clientCert" : {
  	"mode" : "Requested",
  	"CACertFiles" : "rootCA.cer,anotherRootCA.cer",
  	"CATrustStoreFile' : "/path/to/cacerts",
  	"CATrustStorePass' : "changeit"
        }
      }
    }
  }
}
```
{% endcode %}

For more information on how to configure and use client certs, check out our [guide here](./#ssl-client-certs).
