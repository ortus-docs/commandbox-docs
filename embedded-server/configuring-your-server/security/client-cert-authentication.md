# Client Cert Authentication

Client Cert Authentication is common for internal government sites and involves the user installing a private certificate on their PC which the server asks the browser to send while negotiation the SSL connection.  Client certs are a PKI cert, and often times are stored on a CAC which is a physical card issued to the user.

It is a requirement that the user hit your site via SSL in order for their browser to send their client cert OR for you to opt into accepting upstream client cert HTTP headers from a proxy or web server that is negotiating your SSL connection.  It is recommended you configure your server to either disable HTTP or automatically redirect all HTTP traffic to HTTPS, which can be enabled with the `web.SSL.forceSSLRedirect` setting.

### Accepting Client Certs

Configuring your SSL connection to accept or require client certs is covered in the [SSL Client Cert](../bindings/ssl-client-certs.md) portion of the docs.  It's important to note configuring your server to accept SSL client certs and using these certs to authenticate a user two different things.  It's possible to accept (or even require) your client (browser) to send a client cert in your `web.ssl.clientCert` settings but not use the authentication mechanism to protect parts of your site automatically.  Any time a browser has sent a client cert, you will be able to access those details from the `CGI` and `request` scopes to do your own custom authentication.  It is also possible to use Client Cert authentication but not have. &#x20;

### Client Cert Renegotiation

If you have configured your SSL Client Cert settings to accept, but not require client certs, this will allow a user to visit your site and not be challenged to provide a cert until they hit a page that your `authPredicate` kicks in and requires authentication.  Only then will the user's browser prompt them for a cert. &#x20;

```
server set web.security.clientCert.SSLRenegotiationEnable=true
```

SSL Renegotiation, when enabled, will **disable HTTP/2 AND TLSv1.3**.  Both of these are not compatible with SSL Renegotiation. &#x20;

### Trust Upstream Client Cert Headers

Even if CommandBox is not using SSL directly, but is sitting behind a proxy or web server that takes care of negotiating the client cert, you can still use client cert auth to protect pages on your site if you configure CommandBox to trust any `SSL_CLIENT_CERT` HTTP request header.

```bash
server set web.security.clientCert.trustUpstreamHeaders=true
```

Only enable this setting if you TRUST your upstream proxy and you know it will overwrite any potentially-malicious `SSL_CLIENT_CERT` header from the client.  SSL Certs received through this header will appear in the `CGI` and `request` scope and will be trusted the same as a client cert directly negotiated through CommandBox, but there will be NO trust chain verification.  The header will be trusted blindly from your upstream server.  This setting is off by default.

### Enable Client Cert Authentication

You can enable the client cert auth mechanism like so:

```bash
server set web.security.clientCert.enable=true
```

Once enabled, all pages on your site will be secured if you have no `authPredicate` configured.  Otherwise, only pages that match the predicate will be secured.

{% hint style="info" %}
For [Multi-Site](../../multi-site-support/), any client cert auth settings can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.  Just note, client cert auth settings will be shared by all sites using the same SSL binding.
{% endhint %}



### Validate Incoming Certs

By default, when you enable client cert auth, CommandBox simply checks that _some client cert is present_.  Remember, your SSL settings require you to provide a Trust Store or a list of trusted root CAs and only client certs which are trusted by one of those CAs will be negotiated.  This may be enough security for you right there if you are only allowing certs signed/trusted by a specific root cert.  The client cert trust store requires you to explicitly provide every single CA you want trusted.  It does NOT use your Operating system's trust store, nor does it use Java's trust store, nor does it use Lucee Servers. &#x20;

If a request is authorized via a client cert, the Subject distinguished name of the client cert will be present in `cgi.remote_user` just like basic auth works.

#### Filter Subject Distinguished Name (SDN)

You can filter down a subset of client certs you want to be accepted for client cert auth by specifying a full or partial subject DN.  This will not limit the certs which are negotiated and appear in the `CGI` scope.  It will only limit the certs which are allowed to provide authentication for pages secured by the `authPredicate`.  A Subject Distinguished Name (SDN) contains one or more parts known as Relative Distinguished Names, or RDNs.  They follow an LDAP name RFC and look like this:

```
CN=Brad Wood, O=Ortus, OU=Dev
```

You may provide a single Subject DN ...

```javascript
server set web.security.clientCert.subjectDNs="O=Walmart"
```

or an array of Subject DNs to filter on...

```
server set web.security.clientCert.subjectDNs=["CN=Brad","CN=Bob, O=Walmart"]
```

The RDNs can be in any order and you can supply a PARTIAL Subject DN.  CommandBox will match all RDNs regardless of order or case sensitivity.  You are not required to provide the full Subject DN, but all RDNs you provide MUST be present in the incoming cert or it will be rejected.

#### Filter _Issuer Distinguished Name (IDN)_

You can _also_ filter down a subset of client certs you want to be accepted for client cert auth by specifying a full or partial Issuer DN.  This will not limit the certs which are negotiated and appear in the `CGI` scope.  It will only limit the certs which are allowed to provide authentication for pages secured by the `authPredicate`.  An Issuer Distinguished Name (IDN) look and work the same as Subject DNs.

You may provide a single Issuer DN ...

```javascript
server set web.security.clientCert.issuerDNs="CN=GeoTrust TLS RSA CA G1, O=DigiCert Inc, OU=www.digicert.com"
```

or an array of Issuer DNs to filter on...

```
server set web.security.clientCert.issuerDNs=["CN=My CA","O=Verisign"]
```

The RDNs can be in any order and you can supply a PARTIAL Issuer DN.  CommandBox will match all RDNs regardless of order or case sensitivity.  You are not required to provide the full Issuer DN, but all RDSs you provide MUST be present in the incoming cert or it will be rejected.

If you require more specific parsing of the cert pieces, then you’ll need to disable the built in security in CommandBox and write your own code that runs on every request and parses the CGI variables manually to decide if you accept the cert.

### `server.json` Configuration

Here is a look at  what your full config could look like in your `server.json`.  Note this doesn't show the `web.ssl.clientCert` settings which are [covered here](../bindings/ssl-client-certs.md).

```javascript
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



