# Legacy SSL Server Cert syntax

{% hint style="danger" %}
The syntax on this page is deprecated.  It will still work for the foreseeable future, but we recommend using the [new `bindings` object](./) instead which is much more powerful and allows multiple bindings.
{% endhint %}

Turning on SSL in your web server will will enable SSL without an approved SSL certificate. If you need an official certificate so you don't have to confirm your SSL connection you can add these entries

```bash
server set web.SSL.certFile=/path/to/dev_mydomain_ext.crt
server set web.SSL.keyFile=/path/to/dev_mydomain_ext.key
server set web.SSL.keyPass=myPass
```

The cert file and private key can be a PEM encoded file, or a DER-format binary file.

You can also use a PFX file (PKCS #8) by specifying it in the `web.ssl.certFile` setting and then put the password for the PFX file in the `web.ssl.keyPass` setting. You won't use the `web.ssl.keyFile` setting for a PFX since the private key is contained in the main file.

{% hint style="info" %}
For [Multi-Site](../../multi-site-support/), SSL Server Cert settings can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.
{% endhint %}



## Generating a Server Cert

Although free certificates are available (e.g LetsEncrypt) this is not very convenient, because these certs are valid only for three months. Automatic renewal it is difficult if your dev site is not accessible from the web. For a few dollars a year (< 10) you can apply for a domain validated certificate from companies like Comodo, RapidSSL, Trustwave, Digicert, Geotrust and others or a reseller for these certs. For a domain validated certificate you need a valid domain which is under your control which means (depending on provider):

* mail is sent to domain owner
* or mail is sent to well-known administrative contact in the domain, e.g. (admin@, postmaster@, etc.)
* or you can publish a DNS TXT record

So, now you have a valid domain, you have to generate a SSL key and a SSL Certificate Signing Request. With the CSR you can apply for the certificate. Generating a key and CSR with openSSL

```
openssl req -utf8 -nodes -sha256 -newkey rsa:2048 -keyout dev_mydomain_com.key -out dev_mydomain_com.csr
```

This will generate output and some questions, and will finally result in a key file named `dev_mydomain_com.key` and a certificate signing request (csr) named `dev_mydomain_com.csr`

```
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:NL
State or Province Name (full name) [Some-State]:YourState
Locality Name (eg, city) []:YourCity
Organization Name (eg, company) [Internet Widgits Pty Ltd]:YourCompany
Organizational Unit Name (eg, section) []:IT
Common Name (e.g. server FQDN or YOUR name) []:dev.mydomain.com
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

You have to enter Country Name, State and City. Organization Name is preferably the same as the domain owner. Organizational Unit Name will not be checked, so enter something simple such as ICT Common Name is the host name for your site, such as `dev.mydomain.com` You can skip Email Address and optional company name. For development you don't need a challenge password, which means your key file is NOT protected. But don't give this key to others or protect it with a challenge password. If you protect your key you have to `server set web.SSL.keyPass=MyChallengePassword` Now you have a CSR, which you can submit at your SSL provider. They will send you a certificate file (\*.csr), and probably one or more intermediate certificates. Create a new `my.csr` file and copy everything from your certificate file into it, and append the intermediate certificate(s). Now you have a valid `my.csr` certificate file and a key file. Place both files in a location accessible for your CommandBox and enter the corresponding paths to `web.SSL.certFile` and `web.SSL.keyFile`
