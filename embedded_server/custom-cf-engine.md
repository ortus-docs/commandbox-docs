# Create a custom CF engine
This is probably the most work, but allows you to customize every nook and cranny of your CF engine.  You can save your own custom WAR file and use it to start your servers.  When you use the `cfengine` parameter to the `server start` command, you're probably used to pointing it to the [ForgeBox slugs maintained by Ortus](https://www.forgebox.io/type/cf-engines).  The `cfengine` parameter can be any valid CommandBox endpoint ID though including a local file/folder, Git repo, HTTP(s) URL, or custom ForgeBox entry.  So let's say you create a custom ColdFusion 11 WAR that comes pre-loaded with all the settings your app needs and you place it on a shared network drive or web site for your fellow developers.  Just put this in the server.json:

```js
{
  "cfengine" : "/l[ocal/path/to/engine.zip"
  or...
  "cfengine" : "http://www.mysite.com/engine.zip"
}
```
As [documented here](https://ortus.gitbooks.io/commandbox-documentation/content/embedded_server/multi-engine_support.html), the package zip file needs to contain the following two things:

1. **box.json**
2. **Engine.[zip|war]** (file name doesn't matter)

The easiest way to do this is to start your server with one of our Ortus default engines, log into the administrator and make all your changes, and stop the server and navigate to the server's home directory by running this:
```
CommandBox> server info property=serverHomeDirectory | open
```

Zip up the contents of the folder that opens and optionally rename the zip file to have a `.WAR` extension.  The `WEB-INF` folder should be in the root of your zip file.  Then package up that new archive in a new zip long with a `box.json` that minimally has a `version`, `type` (cf-engines), and `slug`.

