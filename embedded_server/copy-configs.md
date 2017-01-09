# Copy Configs on first start
This is probably the best one as it is very flexible and will work on any CF engine regardless of vendor or version.  All CF engines have been standardized to expand their WARs to the same consistent directory structure.  We've also enhanced the `onServerInstall` package script to have access to the server home folder like we did above through the `server info` command.  `onServerInstall` will only fire the first time a server is started and the WAR gets installed. You'll need to `stop`, `server forget` and then `start` again for the event to fire again.  If you want to overwrite the configs every time, use the `onServerStart` event.  

Basically, we'll just copy the XML config files for the server after we've installed the WAR, but before the server actually boots up.  The only drawbacks of this are that the config files differ per engine and per engine version.  If you're starting several different CF engines/versions in the same web root, you'll have some issues with the package script approach since it doesn't have access to the server name being started.

* For **Adobe CF** WARs, the xml config files are located in the WAR here: `/WEB-INF/cfusion/lib/neo.*.xml`
* For the **Lucee server** context, the xml config file is located in the WAR here: `/WEB-INF/lucee-server/context/lucee-server.xml`
* For the **Lucee web** context, the xml config file is located in the WAR here: `/WEB-INF/lucee-web/lucee-web.xml.cfm`

An Adobe CF `box.json` might look like so.  This will copy my datasource XML file from my web root into the WAR the first time I start up the site.
```js
{
  "name":"My app",
  "version":"1.0.0",
  ...
  "scripts":{
    onServerInstall: "cp neo-datasource.xml '`server info property=serverHomeDirectory`/WEB-INF/cfusion/lib/neo-datasource.xml'"
  }
}
```
A Lucee Server `box.json` might look like so.
```js
{
  "name":"My app",
  "version":"1.0.0",
  ...
  "scripts":{
    onServerInstall: "cp lucee-server.xml '`server info property=serverHomeDirectory`/WEB-INF/lucee-server/context/lucee-server.xml'"
  }
}
```

Pay attention to your quotes. My JSON uses double quotes, and the second parameter to the `cp` command is wrapped in single quotes, and contains a [CommandBox expression](https://ortus.gitbooks.io/commandbox-documentation/content/usage/parameters/expressions.html) wrapped in back ticks.  It's also worth noting the server-related package scripts run with their current working directory set to the web root of the server, so any relative paths will be relative to the web root.  The easiest way to get the config files is to make your desired changes via the web-based admin, then stop the server, open the server home, and copy the file.

We recommend keeping config files outside the web root so you don't deploy them on accident.  In that case you'd reference them with something like `../config.xml`.  Read more about [package scripts here](https://ortus.gitbooks.io/commandbox-documentation/content/developing/interceptors/interceptor_based_cli_scripts.html).

