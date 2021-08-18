# web.xml Overrides

When you start an Adobe or Lucee CF Engine, the WAR CommandBox uses has a stock `web.xml` baked into it.  Sometimes you may want to add custom servlets, servlet mappings, etc into your server.  You can override the stock `web.xml` in part or in total with a file of your own which you specify in the `server.json` like so:

```javascript
{
  "app" : {
    "webXMLOverride" : "path/to/web-override.xml"
  }
}
```

The path can be absolute or relative to the `server.json` file.  CommandBox will still load the default `web.xml` from the CF engine, and then it will load your override file _on top_ of the previous settings.  This means additional items in your override will be merged into the existing settings, and servlets with the same name as existing default servlets will be completely overridden and replaced.

Here is a list of all the top level items CommandBox will look for in your `web.xml` file:

* Servlets
* Servlet mappings
* Servlet filters
* Server filter mappings
* Listeners
* Context params
* welcome files _\(only if not in server.json\)_
* error pages _\(only if not in server.json\)_
* mime types
* session config

So if you wanted to replace the default servlet mapping for a Lucee server to also process `.html` files, you could use an override file like this:

```markup
<?xml version="1.0" encoding="utf-8"?><web-app xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" metadata-complete="true" version="2.5" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
<web-app>
    <servlet-mapping>
    <servlet-name>CFMLServlet</servlet-name>
    <url-pattern>*.cfc</url-pattern>
    <url-pattern>*.cfm</url-pattern>
    <url-pattern>*.cfml</url-pattern>
    <url-pattern>*.html</url-pattern>
    <url-pattern>/index.cfc/*</url-pattern>
    <url-pattern>/index.cfm/*</url-pattern>
    <url-pattern>/index.cfml/*</url-pattern>
  </servlet-mapping>
</web-app>
```

## Remove Defaults

The default behavior is to add or update existing configurations.  If you want to _remove_ a servlet or listener from the base file, you can enable the `app.webXMLOverrideForce` flag.

```bash
server set app.webXMLOverrideForce=true
```

This will completely remove any configuration from the `web.xml` that is explicitly provided in your override file.  For example, if the default `web.xml` specifies two servlet filters and you provide an override `web.xml` with one servlet filter and the force flag is true, both of the default servlet filters will be removed in favor of your one servlet filter from your override file.

If you need something more complex than this-- e.g. simply removing default settings without replacing them, you'll need to create a custom CF engine with a default `web.xml` of your own design.

