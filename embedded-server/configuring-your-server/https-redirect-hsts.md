# HTTPS Redirect/HSTS

When using a CommandBox web server in production, you may wish to force your users to visit your site over HTTPS for security \(and for HTTP/2 to work\).  However, it is desirable to still have your web server listening on HTTP so a user just typing your address in his browser can still connect to HTTP and then redirect.

## Automatic HTTPS Redirection

CommandBox can be configured to redirect all HTTP traffic over to HTTPS with the following rule.  The redirect will use a `301` status code and will include the URL and query string.  The redirect will only fire for `GET` HTTP requests since redirecting a form `POST` would lose the form fields \(request body\).

```bash
server set web.SSL.forceSSLRedirect=true
```

Or in the `server.json` as:

{% code title="server.json" %}
```javascript
{
  "web": {
    "ssl" : {
      "enable" : true,
      "port" : 443,
      "forceSSLRedirect" : true
     }
  }
}
```
{% endcode %}

If you want to customize how this redirect happens, you can skip this setting and manually add the following [Server Rule](server-rules/), which is basically what the setting above does automatically:

```javascript
not secure() and method(GET) -> redirect('https://%{LOCAL_SERVER_NAME}%{REQUEST_URL}%{QUERY_STRING}' )
```

## HTTP Strict Transport Security \(HSTS\)

If you want to go one step further, you can add a `Strict-Transport-Security` header to your site.  This instructs the browser to _automatically_ use HTTPS every time the user visits your site again.  With an HSTS header, once your browser has cached the fact that you want HTTPS to always be used, even if the user just types your domain and hits enter, the browser will change it over to HTTPS before sending the request.  This prevents having an initial HTTP request before the redirect kicks in.  

HSTS headers can have a "max age" set, which tells the browser how long to "remember" to use HTTPS.  This defaults to 1 year if you do not override the value. The max age is set in second.  So 31,536,000 is one year.  You can also set whether you want the HSTS header to apply to all sub domains of your site.  The default is `false` for this setting.  

Here is how you enable HSTS.  Only the first `web.SSL.HSTS.enable` setting is required.  The rest are optional.

```bash
server set web.SSL.HSTS.enable=true
server set web.SSL.HSTS.maxAge=31536000
server set web.SSL.HSTS.includeSubDomains=true
```

Or in the `server.json` as:

{% code title="server.json" %}
```javascript
{
  "web": {
    "ssl" : {
      "enable" : true,
      "port" : 443,
      "HSTS" : {
        "enable" : true,
        "maxAge" : 31536000,
        "includeSubDomains" : true
      }
     }
  }
}
```
{% endcode %}

The HSTS header is added via a [Server Rule](server-rules/).  Here is what the rule looks like in case you want to manually add the rule to customize it.

```javascript
set(attribute='%{o,Strict-Transport-Security}', value='max-age=31536000; includeSubDomains')
```

