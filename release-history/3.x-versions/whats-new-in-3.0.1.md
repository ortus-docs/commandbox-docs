# What's New in 3.0.1

Properties weren't being read correctly from the `server.json` file.  If you have been using `server.json`, please double check the format of the file here in our docs:

[http://commandbox.ortusbooks.com/content/embedded\_server/serverjson.html](http://commandbox.ortusbooks.com/content/embedded\_server/serverjson.html)

This fix will make this functionality work as expected:

```
server set web.http.port=8000
server start
```

## &#x20;Upgrading

If you already have 3.0.0 then this fix only affects the CFML bits and is very easy for you to install.  Simply run this command:

```
upgrade
```

If you're still on CommandBox 2.x, check out our [3.0.0 release announcement](https://www.ortussolutions.com/blog/commandbox-300-final-released) to see the cool new stuff.
