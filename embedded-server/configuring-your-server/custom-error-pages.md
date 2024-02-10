# Custom Error Pages

You can customize the error page that CommandBox servers return. You can have a setting for each status code including a default error page to be used if no other setting applies.

Create an `errorPages` object inside the `web` object in your `server.json` where each key is the status code integer or the word `default` and the value is a relative (to the web root) path to be loaded for that status code.

This is what you `server.json` might look like:

```
{
  "web" : {
    "errorPages" : {
      "404" : "/path/to/404.html",
      "500" : "/path/to/500.html",
      "default" : "/path/to/default.html"
    }
  }
}
```

You can set error pages via the `server set` command like this:

```
server set web.errorPages.404=/missing.htm
```

{% hint style="info" %}
For [Multi-Site](../multi-site-support/), custom error page settings can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.
{% endhint %}

## Accessing error variables

If your error page points to a CFM file, you can get access to the original path being accessed for 404s and the error that was thrown for 500s. To see all the request headers that are available, use the following snippet of code:

```javascript
req = getPageContext().getRequest();
names = req.getAttributeNames();
while( names.hasMoreElements() ) {
    name = names.nextElement();
    writeOutput( name & ' = ' & req.getAttribute( name ) & '<br>' );
}
```

An example of getting the original missing path in a 404 in Lucee would look like this:

```javascript
var originalPath = getPageContext().getRequest().getAttribute( "javax.servlet.error.request_uri" );
```

In Adobe ColdFusion, you can access `ServletRequest` Attributes directly via the CGI scope:

```javascript
var originalPath = cgi[ 'javax.servlet.error.request_uri' ];
```

