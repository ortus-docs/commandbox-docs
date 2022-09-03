# Security

It is common to protect pages on your site through your CF code that runs on every request.  This does not protect static files like images, plus it can still expose extra surface area of your site for attack.  CommandBox has a security system to block certain paths on your site at the web server level, so these requests never reach CF.

### Authentication Mechanisms

There are currently two authentication mechanisms you can enable in CommandBox.  Any combination of them can be used to secure the protected portions of your site.   When CommandBox determines that the current page requires authentication, it will check each enabled auth mechanism until it finds one that can validate the user.

#### Basic Authentication

Basic Authentication is built on a simple username/password list and challenges the user to provide credentials in their browser.  [Read more here](basic-authentication.md)

#### Client Cert Authentication

Client Cert auth is common for internal government sites and involves the user installing a private certificate on their PC which the server asks the browser to send while negotiation the SSL connection.  Client certs are a PKI cert, and often times are stored on a CAC which is a physical card issued to the user.

#### More to Come

There are more auth mechanisms we will add in the future including Digest Auth, Header Auth, and SSO.  Contact us if you're wanting to sponsor any of these.

### Auth Predicate

If you do not provide an auth predicate but you enable at least one auth mechanism, ALL PAGES on the entire site will be secured.  This is quite secure, but often times you only want to protect a subfolder such as `/CFIDE/administrator` or `/lucee/admin`.

To limit the scope of what pages required authentication, you can specify an auth predicate, which follows the same syntax as [Server Rules](../server-rules/). &#x20;

```bash
server set web.security.authPredicate="regex( pattern='^/lucee/admin/.*', case-sensitive=false )"
```

We don't recommend using `path()` or `path-prefix()` as they are not case sensitive, which can lead to them being easily worked around if you are on Windows.  Even `regex()` is case-sensitive by default.  Only use a case sensitive predicate matcher if your file system and web server are set to be case sensitive!   If your server is case sensitive, then the example above can be simplified to this:

```bash
server set web.security.authPredicate="path-prefix(/lucee/admin/)"
```

The most common auth predicates match a subfolder, but you are not limited to this.  Anything valid in a server rule predicate can be used here, including HTTP method, headers, remote IP, etc.
