# SSL Client Certs

Accepting a client cert as part of the SSL negotiation is a totally separate idea as using the existence of a client cert to authorize a given request. This is why the settings are in two different place in the `server.json`. You can have CommandBox ask (or require!) the browser to hand over a client cert, but all that does is set a bunch of CGI variables and servlet request attributes to document what the incoming cert is. The same client cert details can also be passed from an upstream proxy which negotiated the SSL connection and the details can be sent via HTTP headers to CommandBox to use for request authorization. Therefore, CommandBox can enable client certs at the SSL level and NOT enable request authorization, or it can disable its own SSL client cert negotiation but still perform request-level authorization based purely on upstream headers. The two features are not mutually dependent.  To read how to have CommandBox automatically protect portions of your site based on a client cert, [read here](../security/client-cert-authentication.md).

### Client Cert Negotiation

To enable client certs to be negotiated by CommandBox SSL must be enabled, and then `web.ssl.clientCert.mode` can be set to one of the following options:

* **NOT\_REQUESTED** - (default) SSL client authentication is not requested. (same as “ignore” in IIS)
* **REQUESTED** - SSL client authentication is requested but not required. (same as “request” in IIS)
* **REQUIRED** - SSL client authentication is required. (same as “require” in IIS)

```bash
server set web.ssl.clientCert.mode=required
```

### Trust Store

CommandBox will NOT use your OS trust store nor your Java trust store, nor Lucee Server's trust store by default to establish a trust chain for incoming client certs. You must specify the trusted root CA certs that you want trusted. ONLY client certs trusted by one of these CAs will be accepted. Any intermediate certs must also be included. You can specify a comma-delimited list of absolute or relative paths OR an array of strings in the `web.ssl.clientCert.CACertFiles` setting that point to any number of public keys in a DER format (typically .crt or .cer extension)

```bash
server set web.ssl.clientCert.CACertFiles=digitcert.crt,verisign.crt
```

or

```bash
server set web.ssl.clientCert.CACertFiles=["digitcert.crt","verisign.crt"]
```

If you have a large number of trusted CA’s, you can specify a JKS trust store (same format as Java’s `cacerts` file) along with the password for the store in the `web.ssl.clientCert.CATrustStoreFile` and `web.ssl.clientCert.CATrustStorePass` file. All CAs in that trust store will be used to validate incoming client certs.

```
server set web.ssl.clientCert.CATrustStoreFile=cacerts
server set web.ssl.clientCert.CATrustStorePass=changeit
```

{% hint style="warning" %}
The user's browser will ONLY be able to send client certs signed/trusted by one of the CAs you specify.  All other certs will be rejected.  If your mode is set to REQUIRED, that means your user would never be able to reach any CF pages at all (unless you are using SSL renegotiation which is [covered here](../security/client-cert-authentication.md#client-cert-renegotiation).
{% endhint %}

### `server.json` Configuration

Here's an example of what your config could look like:

```javascript
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
	"CATrustStoreFile' : "cacerts",
	"CATrustStorePass' : "changeit"
      }
    }
  }
}
```

If both a trust store AND individual trusted CA's are provided, CommandBox will use all of them.

### Accessing the Client Cert

If the user's browser sends a client cert which is trusted by one of your trusted CAs, then the details of the cert will be available for Client Cert Authentication as [documented here](../security/client-cert-authentication.md).  There are also a number of `CGI` and `request` variables automatically made available to you any time a client cert was presented.  These variables will be available on every page, not just the first page where they select their cert.

#### CGI Scope

When a valid client cert is present, the following `CGI` variables will be available to your CF code. If client certs are “requested”, these CGI variables will be wiped from any upstream sources so you can always trust them to be set from CommandBox, even if they are empty. There are several duplicate values since we are mimicking IIS, Apache, and Nginx client cert implementations for maximum compatibility.

* `CGI.SSL_CLIENT_CERT` - PEM-encoded cert (base 64 string)
* `CGI.X_ARR_CLIENTCERT` - PEM-encoded cert (base 64 string)
* `CGI.SSL_CLIENT_S_DN` - The Subject distinguished name of the client cert (CN=foo, O=bar, OU=baz)
* `CGI.CERT_SUBJECT` - The Subject distinguished name of the client cert (CN=foo, O=bar, OU=baz)
* `CGI.CERT_KEYSIZE` - The key size of the negotiated SSL connection
* `CGI.CERT_SERIALNUMBER` - The serial number of the cert in the format `91-7e-5f-a5-b2-20-a1-8b-4c-d0-40-3b-1c-a1-a8-58`
* `CGI.SSL_CLIENT_M_SERIAL` - The serial number of the cert in the format `91-7e-5f-a5-b2-20-a1-8b-4c-d0-40-3b-1c-a1-a8-58`
* `CGI.SSL_CLIENT_I_DN` - The Issuer distinguished name of the client cert (CN=foo, O=bar, OU=baz)
* `CGI.CERT_ISSUER` - The Issuer distinguished name of the client cert (CN=foo, O=bar, OU=baz)
* `CGI.SSL_CLIENT_VERIFY` - Matches Apache HTTP. Values will be "SUCCESS" or "NONE"
* `CGI.SSL_SESSION_ID` - Unique ID of the SSL connection

Note, some of these vars may not appear when you cfdump out the `CGI` scope, but will still be available when you specify `cgi.var_name`&#x20;

&#x20;Due to some differences between Adobe ColdFusion and Lucee Server, the above variables are loaded as HTTP Request Headers in Lucee and Servlet Request Attributes in Adobe ColdFusion.  You can access them in either engine though through the `CGI` scope, but they may be case sensitive (always upper case)

#### Request Scope

The following servlet request attribute will also be set, which are accessible in a **case sensitive manner** from both Adobe and Lucee Server `request` scopes:

* `javax.servlet.request.cipher_suite` - The cipher suite of the SSL connection such as `TLS_AES_128_GCM_SHA256`
* `javax.servlet.request.key_size` - The key size of the negotiated SSL connection
* `javax.servlet.request.ssl_session_id` - Unique ID of the SSL connection
* `javax.servlet.request.X509Certificate` - An array of `java.security.cert.X509Certificate` instances representing the certificate chain. These cert objects contain the same data as the PEM-encoded headers.
* `javax.servlet.request.X509Certificate.issuerDNMap` - A struct containing all the parts of the Issuer distinguished name. So for (`CN=foo, O=bar`) there would be a key for `CN` and `O` in the struct with respective values of `foo`, and `bar`. _USE THIS instead of regex on the full Issuer DN because this map is generated by following the exact RFC for LDAP names._
* `javax.servlet.request.X509Certificate.subjectDNMap` - A struct containing all the parts of the Subject distinguished name. So for (`CN=foo, O=bar`) there would be a key for `CN` and `O` in the struct with respective values of `foo`, and `bar`. _USE THIS instead of regex on the full Subject DN because this map is generated by following the exact RFC for LDAP_ names.

Access these request vars like so in CFML

```javascript
myCertArray = request['javax.servlet.request.X509Certificate'];
SubjectRDNs = request['javax.servlet.request.X509Certificate.subjectDNMap'];
```

Note using quoted string inside of bracket notation is required not only due to the periods in the variable name, but also to preserve the case-sensitivity of them.

### **Upstream SSL headers**

All of the CGI and request vars above can also be present even if CommandBox’s built in client cert negotiation is not enabled if you have PEM-encode headers being sent from your upstream web server. However, you must explicitly tell CommandBox to trust these headers. Only do this if you trust the upstream proxy to always set trusted values into these headers. Enable this by setting the `web.security.clientCert.trustUpstreamHeaders` key by setting it to `true`. Them CommandBox will look for the `SSL_CLIENT_CERT`, `SSL_CIPHER`, and `SSL_SESSION_ID` HTTP request headers and associate those certs with the request just as though CommandBox had negotiated them itself.

```javascript
{
  "web" : {
    "security" : {
      "clientCert" : {
        "trustUpstreamHeaders" : true
      }
    }
  }
}
```
