# What's New in 5.6.0

#### Library Updates

We know this stuff may seem boring, but it's super important to ensure you stay safe and secure on the latest versions of our bundled libraries. We updated the following libs in this release:

* `org.lucee:lucee` 5.3.9.141 -> **5.3.9.160**
* `io.undertow:undertow-servlet` 2.2.17.Final -> **2.2.19.Final**
* `io.undertow:undertow-websockets-jsr` 2.2.17.Final -> **2.2.19.Final**
* `net.minidev:json-smart-mini` 1.0.8 -> **1.3.2**
* `commons-cli:commons-cli` 1.2 -> **1.5.0**
* `org.jooq:joox` 1.2.0 -> **1.6.2**
* `org.apache.logging.log4j:log4j-slf4j-impl` 2.17.1 -> **2.18.0**
* `org.apache.logging.log4j:log4j-core` 2.17.1 -> **2.18.0**
* `org.jboss.logging:jboss-logging` 3.4.1.Final -> **3.4.3.Final**

#### Lots of Bug Fixes

Over half of the tickets in this release were bug fixes to keep the CLI running smoothly on all operating systems.  You can check out the full list of ticket below to see the screws we tightened.&#x20;

#### New Server Security System

CommandBox servers have an exciting new weapon in their arsenal, and that is a new system of security that allows you to protect certain parts of your site from the general public.  This could be CF admins, private dashboards, or a subfolder of sensitive files. &#x20;

You'll find a new section in the `server.json` called `web.security` where these settings live.  You can leverage the power of our Server Rule predicates to match whatever requests you want to secure, based on folder, HTTP method, remote IP, HTTP headers and more.&#x20;

```
{
  "web" : {
    "security" : {
      "realm" : "My Realm",
      "authPredicate" : "path-prefix( /lucee/admin/ ) and not equals('%{REMOTE_IP}', 127.0.0.1)"
    }
  }
}
```

That `authPredicate` would require authorization for any pages in the Lucee admin unless you were on localhost. NOTE: path-prefix is case sensitive, so on Windows you'd want to use a `regex()` based check such as `regex( pattern='^/lucee/admin/.*', case-sensitive=false )`

When a request is marked as requiring authentication, you can enable one or more auth mechanisms to challenge the user as discussed below.

Read more on Server Security Here.

[https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/security](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/security)

#### Basic Auth Security

CommandBox has supported basic auth for a while, but it was a simple all-or-nothing implementation.  Basic auth has been revamped and rolled into the new security system.  If no `authPredicate` is defined, it will still apply to the whole site.  But when an `authPredicate` is declared in your `server.json`, it will only kick in for those pages. &#x20;

We've also moved the basic auth settings in `server.json` to here:

```
{
    "web":{
        "security" : {
            "realm" : "My Realm",
            "authPredicate" : "regex( pattern='^/lucee/admin/.*', case-sensitive=false )"
            "basicAuth":{
                "users":{
                    "brad":"pass",
                    "luis":"pass2"
                },
                "enable":"true"
            }
        }
    }
}
```

Don't worry, the old location still works too for now.  We won't remove support for it until the next major release of CommandBox.  If both the settings exist (Ex: `web.basicAuth.enable` and `web.security.basicAuth.enable`), the new location will be given precedence.

Read more on Basic Auth Security Here.

[https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/security/basic-authentication](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/security/basic-authentication)

#### Client Cert Security

Adding support for client SSL certs was one of our largest undertakings and is a very exciting new feature for government shops who use PKI based authentication, often times in the form of DoD CAC (cards) which are physical cards containing a private PKI cert that identifies the user.  This feature was one of the last reasons to need IIS or Apache in your mix, but now CommandBox can do it all!

Client certs have two part-- first is the ability of the web server to prompt the user's browser to ask for a client cert to send.  This requires configuring a trust store or a list of trusted CA certs to accept.  When the user sends a cert, it automatically makes a number of CGI and request variables available to your CF code.  You can configure your SSL connection to accept or require client certs like so:

```
{
  "web" : {
    "ssl" : {
      "enable" : true,
      "clientCert" : {
	"mode" : "Requested",
	"CACertFiles" : "rootCA.cer,anotherRootCA.cer",
	// OR...
	"CACertFiles" : [
          "rootCA.cer",
          "anotherRootCA.cer"
	],
        // OR...
	"CATrustStoreFile' : "cacerts",
	"CATrustStorePass' : "changeit"
      }
    }
  }
}
```

Some of the CGI variables which are automatically created when a client cert is present are

* `CGI.SSL_CLIENT_CERT` - PEM-encoded cert (base 64 string)
* `CGI.CERT_SUBJECT` - The Subject distinguished name of the client cert (CN=foo, O=bar, OU=baz)
* `CGI.CERT_SERIALNUMBER` - The serial number of the cert in the format `91-7e-5f-a5-b2-20-a1-8b-4c-d0-40-3b-1c-a1-a8-58`
* `CGI.CERT_ISSUER` - The Issuer distinguished name of the client cert (CN=foo, O=bar, OU=baz)
* `CGI.SSL_CLIENT_VERIFY` - Matches Apache HTTP. Values will be "SUCCESS" or "NONE"

The second part of client certs is the ability to use that client cert information as an authentication mechanism to enforce your `authPredicate` automatically.  (When CommandBox's security system is unable to authorize a user, it stops the request before it ever even reaches CF!)

When authorizing based on client certs, you can have 4 levels of checks:

* Any user with a cert is allowed. (Remember, the client cert must always be trusted by one of your configured trusted CA certs)
* Subject Distinguished Name (DN) matches one or more complete or partial DNs you specify
* Issuer Distinguished Name (DN) matches one or more complete or partial DNs you specify
* Or disable the `web.security.clientCert.enabled` setting and allow all requests to reach CF where you can write your own checks.

CommandBox also supports SSL Renegotiation which allows you to not force the client cert right away until the user gets to a page on the site that kicks in the `authPredicate` and then their browser will prompt them then.  This is a popular configuration since the user can hit your login page first and then be prompted for their cert once they login. &#x20;

The configuration for all this looks like this:

```
{
  "web" : {
    "security" : {
      "realm" : "My Realm",
      "authPredicate" : "path-prefix( /admin )"
      "clientCert" : {
        "enable" : true,
        "SSLRenegotiationEnable":true,
        "subjectDNs" : "O=Ortus, OU=Marketing",
        "issuerDNs" : [
          "O=Verisign",
          "CN=Bob, O=Walmart",
          "CN=GeoTrust TLS RSA CA G1, O=DigiCert Inc, OU=www.digicert.com"
        ]
      }
    }
  }
}
```

Read more on Client Cert Auth security Here.

[https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/security/client-cert-authentication](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/security/client-cert-authentication)

#### Task Runner `loadModules()`

As Task Runners become more popular and people combine them with more modules to perform their operations, you run into the need to load a list of modules all at the same time which may have interdependencies.  There is now a new `loadModules()` method available to Task Runners which accepts an array of module paths.  Each module is first registered, and then each module is activated. &#x20;

```
loadModules(
    directoryList( path=resolvePath( 'modules/' ), type='dir' )
);
```

Read more here:

[https://commandbox.ortusbooks.com/task-runners/loading-ad-hoc-modules#loading-multiple-modules](https://commandbox.ortusbooks.com/task-runners/loading-ad-hoc-modules#loading-multiple-modules)

### Release notes
