# URL Rewrites

Once you start  using the embedded server for your development projects, you may wish you enable URL rewriting.  Rewrites are used by most popular frameworks to do things like add the `index.cfm` back into SES URLs.  

You may be used to configuring URL rewrites in Apache or IIS, but rewrites are also possible in CommandBox's embedded server via a [Tuckey servlet filter](http://tuckey.org/urlrewrite/).

## Default Rules 

We've already added the required jars and created a default rewrite [XML file](http://urlrewritefilter.googlecode.com/svn/trunk/src/doc/manual/4.0/index.html#filterparams) that will work out-of-the-box with the ColdBox MVC Platform.  To enable rewrites, start your server with the `--rewritesEnable` flag.

```bash
start --rewritesEnable
```

Now URLs like 
```
http://localhost/index.cfm/main
```
can now simply be 
```
http://localhost/main
```

In `server.json`

```bash
server set rewritesEnable=true
server show rewritesEnable
```

> **info** The default rewrite file can be found in `~\.CommandBox\cfml\system\config\urlrewrite.xml`

## Custom Rules 

If you want to customize your rewrite rules, just create your own XML file and specify it when starting the server with the `rewritesConfig` parameter.  Here we have a simple rewrite rule that redirects  `/foo` to `/index.cfm`


**customRewrites.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<urlrewrite>
	<!-- this will redirect the user from /foo to /index.cfm -->
	<rule>
		<from>^/foo$</from>
		<to type="redirect">/index.cfm</to>
	</rule>
	<!-- internally redirect the requested URL from /gallery to /index.cfm?page=gallery with query string appended -->
	<rule>
		<from>^/gallery</from>
		<to type="passthrough" qsappend="true">/index.cfm?page=gallery</to>
	</rule>

</urlrewrite>
```

Then, fire up your server with its custom rewrite rules:
```bash
start --rewritesEnable rewritesConfig=customRewrites.xml
```
 
In `server.json`

```bash
server set rewritesEnable=true
server set rewritesConfig=customRewrites.xml


server show rewritesEnable
server show rewritesConfig
```
 
>**info** For more information on custom rewrite rules, consult the [Tuckey docs](http://urlrewritefilter.googlecode.com/svn/trunk/src/doc/manual/4.0/index.html#filterparams).
