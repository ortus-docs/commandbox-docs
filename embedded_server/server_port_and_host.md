# Server Port and Host

The `start` command will scan your system and find a random port that is not currently in use to start the server on.  This ensures that multiple embedded servers can run at the same time on the same host without collisions.  Ensure any redirects in your applications take the port into account.

## Any Port in the Storm

You may want to set a specific port to use-- even port 80 if nothing else is using it.  Pass the HTTP port parameter to the start command like so:

```bash
 start port=8080
```

It is also possible to save the default port in your `server.json`.  Add a `web.http.port` property, or issue the following command:

```bash
server set web.http.port=8080
server show web.http.port
```

Now every time you `start` your server, the same port will be used.

If the server won't start or is unreachable, make sure it's port is free with your operating system's `netstat` command.  On Unix-based OS's:

```bash
 $> netstat -pan | grep 80
```

### HTTPS

You can start your server to listen for SSL connections too.

```bash
start SSLEnable=true SSLPort=443
```

```bash
server set web.SSL.enable=true
server set web.SSL.port=8080
server show web.SSL.enable
server show web.SSL.port
```
This will enable SSL without an approved SSL certificate. If you need an official certificate so you don't have to confirm your SSL connection you can add these entries

```bash
server set web.SSL.certFile=/path/to/dev_mydomain_ext.crt
server set web.SSL.keyFile=/path/to/dev_mydomain_ext.key
```
Although free certificates are available (e.g LetsEncrypt) this is not very convenient, because these certs are valid only for three months. Automatic renewal it is difficult if your dev site is not accessible from the web.
For a few dollars a year (< 10) you can apply for a domainvalidated certificate from companies like Comodo, RapidSSL, Trustwave, Digicert, Geotrust and others or a reseller for these certs.
For a domein validated certificate you need a valid domain which is under your control which means (depending on provider):
- mail is sent to domain owner
- or mail is sent to well-known administrative contact in the domain, e.g. (admin@, postmaster@, etc.)
- or you can publish a DNS TXT record

So, now you have a valid domain, you have to generate a SSL key and a SSL Certificate Signing Request. With the CSR you can apply for the certificate.
Generating a key and CSR with openSSL
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
You have to enter Country Name, State and City. Organization Name is preferably the same as the domain owner.
Organizational Unit Name will not be checked, so enter something simple such as ICT
Common Name is the hostname for your site, such as dev.mydomain.com
You can skip Email Adress and optional company name. 
For development you don't need a challenge password, which means your key file is NOT protected. But don't give this key to others or protect it with a challenge password. If you protect your key you have to `server set web.SSL.keyPass=MyChallengePassword` 
Now you have a CSR, which you can submit at your SSL provider.
They will send you a certificate file (*.csr), and probably one or more intermediate certificates.
Create a new my.csr file and copy everything from your certificate file into it, and append the intermediate certificate(s).
Now you have a valid my.csr certificate file and a key file. Place both files in a location accessible for your commandbox and enter the corresponding paths to web.SSL.certFile and web.SSL.keyFile

### AJP

You can start your server to listen for AJP connections too.

```bash
start AJPEnable=true AJPPort=8009
```

```bash
server set web.AJP.enable=true
server set web.AJP.port=8009
server show web.AJP.enable
server show web.AJP.port
```

## A Gracious Host

Your application may rely on a specific host name other than the default of `127.0.0.1`.  You can set the host to anything you like, but you must add a `host` file entry that resolves your host name to an IP address assigned to your network adapter \(usually 127.0.0.1\)

```bash
 start host=mycoolsite.local
```

If you have multiple IP addresses assigned to your PC, you can bind the server to a specific IP using the `host` parameter.

```bash
 start host=192.168.10.15 port=80
```

A server configuration can only have one host entry. If you require your server to be available on multiple IP addresses of the machine it runs on, you can set the host to 0.0.0.0. This will effectively bind the server to all network interfaces (including local).

```bash
 start host=0.0.0.0 port=80
```

Or save in `server.json`

```bash
server set web.host=mycoolsite.local
server show web.host
```

## Customize URL that opens for server

By default, CommandBox will open your browser with the host and port of the server.  You can customize the exact URL that opens.  This setting will be appended to the current host and port.
```
server set openBrowserURL=/bar.cfm
```

Or you can completely override the URL if your setting starts with `http://`.

```
server set openBrowserURL=http://127.0.0.1:59715/test.cfm
```


