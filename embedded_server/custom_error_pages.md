# Custom Error Pages

You can customize the error page that CommandBox servers return.  You can have a setting for each status code including a default error page to be used if no other setting applies.

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
server set web.aliases.404=/missing.htm
```

## Accessing error variables
If your error page points to a CFM file, you can get access to the original path being accessed for 404s and the error that was thrown for 500s.  To see all the request headers that are available, use the following snippet of code:

```req = getPageContext().getRequest();
names = req.getAttributeNames();
while( names.hasMoreElements() ) {
	name = names.nextElement();
	writeDump( name & ' = ' & req.getAttribute( name ) );
}
```

An example of getting the original missing path in a 404 would look like this:
```
var originalPath = getPageContext().getRequest().getAttribute( "javax.servlet.error.request_uri" );
```