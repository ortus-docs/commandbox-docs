# URL Rewrites

Once you start  using the embedded server for your development projects, you may wish you enable URL rewriting.  Rewrites are used by most popular frameworks to do things like add the `index.cfm` back into your URLs.  

You may be used to configuring URL rewrites in Apache or IIS, but rewrites are also possible in CommandBox's embedded server via a [Tuckey servlet filter](http://tuckey.org/urlrewrite/).

## Default Rules 

We've already added the required jars and created a default rewrite [XML file](http://urlrewritefilter.googlecode.com/svn/trunk/src/doc/manual/4.0/index.html#filterparams) that will work out-of-the-box with the ColdBox MVC Platform.  To enable rewrites, start your server with the `--rewritesEnabled` flag.

```bash
start --rewritesEnabled
```

Now URLs like 
```
http://localhost/index.cfm/main
```
can now simply be 
```
http://localhost/main
```

The default rewrite file can be found in 

## Custom Rules 

If you want to customize 