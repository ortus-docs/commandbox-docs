# Adobe CF Features

## CFPM

Adobe ColdFusion 2021 and later have a built in command in their server home called `cfpm` (ColdFusion Package Manager) used to install modules into the core of the ACF engine. &#x20;

CommandBox provides a passthrough command called `cfpm` which will locate the Adobe server and invoke the correct cfpm.bat or cfpm.sh script, passing along whatever arguments you've typed.

```bash
CommandBox> cfpm install orm,debugger,pdf
```

The CommandBox `cfpm` command will use the current working directory of the shell, or intercept data, if run from inside of a package script or server script to figure out which server to apply to.  It will also ensure the `JAVA_HOME` env var is set for the process.

If you need to force `cfpm` to act upon a specfific CommandBox server, set the following env var before running it:

```bash
set CFPM_SERVER=my-CommandBox-server-name
cfpm install orm
```

## Script Alias

CommandBox adds a `/cf_scripts/scripts` alias for you any time you start an Adobe CF server.  This alias points to the same folders in the root of the Adobe WAR.  If you set a custom scripts src path in the CF administrator then you'll want to ensure CommandBox uses the expected alias.

There is a setting called `web.adobeScriptsAlias` which allows you to control the public, web-accessible path to the scripts folder that CommandBox creates for you.

```bash
server set web.adobeScriptAlias=/my-scripts
```

{% hint style="info" %}
For [Multi-Site](../multi-site-support/), Adobe's Script Alias settings can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.
{% endhint %}

If you want to entirely disable the Adobe script alias, you can set that setting to an empty string. &#x20;

```
server set web.adobeScriptAlias=
```

If you're using CFConfig as well to manage your Adobe CF configuration, note that CFConfig is smart enough to adjust automatically to this setting.

* If a `web.adobeScriptsAlias` is set in CommandBox and there is not a custom CFConfig setting, then the same alias will be set into ACF via CFConfig.
* If CFConfig has a custom `CFFormScriptDirectory` setting but CommandBox doesn’t have a custom `adobeScriptsAlias` setting, then default CommandBox’s alias to be the one set in CFConfig.

So basically, you can set the script directory either in CFConfig or in your `server.json` and the other will follow suite with no additional effort.
