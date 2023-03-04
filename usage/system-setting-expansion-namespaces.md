# System Setting Expansion Namespaces

The default namespace when using the `${foo}` system setting expansion syntax is box environment variable, Java system properties, and OS environment variables.

It is also possible to leverage built-in namespaces to allow expansions that reference:

* **server.json** properties
* **box.json** properties
* arbitrary JSON file properties
* Config settings (like the `config show` command)
* Server info properties (like the `server info property=name` command)
* Other properties in the same JSON file

## `server.json` properties

If the current working directory of the shell contains a `server.json` file, you can reference any property in it with the `serverjson.` namespace.:

```javascript
${serverjson.name}
${serverjson.app.cfengine}
${serverjson.web.http.port}
${serverjson.trayEnable:defaultValue}
```

if you have a server json file with a non-default name such as `server-custom.json` then you can access it with the `serverjson.` namespace.:

```javascript
${serverjson.name@server-custom.json}
${serverjson.app.cfengine@server-custom.json}
${serverjson.web.http.port@server-custom.json}
${serverjson.trayEnable@server-custom.json:defaultValue}
```

## `box.json` properties

If the current working directory of the shell contains a `box.json` file, you can reference any property in it with the `boxjson.` namespace.:

```javascript
${boxjson.name}
${boxjson.slug}
${boxjson.testbox.runner}
${boxjson.description:defaultValue}
```

## Arbitrary JSON file properties

You can can reference properties from any JSON file with either a relative path (to the current working directory) or an absolute path with the `json.` namespace..

```javascript
${json.myProperty.name@myFile.json}
${json.myProperty.name@/path/to/myFile.json:defaultValue}
```

## Config settings

You can expand any valid config setting with the `configsetting.` namespace. So getting the same value you get when you run the command

```bash
config show endpoints.forgebox.apitoken
```

can be expanded like this:

```javascript
${configsetting.endpoints.forgebox.apitoken}
${configsetting.endpoints.forgebox.apitoken:defaultValue}
```

## Server info properties

You can expand any valid server info property with the `serverinfo.` namespace. So getting the same value you get when you run the command

```bash
server info property=serverHomeDirectory
```

can be expanded like this:

```javascript
${serverinfo.serverHomeDirectory}
${serverinfo.serverHomeDirectory:defaultValue}
```

To see all the possible properties you can acess in the `serverinfo` namespace, run this command:

```bash
server info --json
```

Every key in the outputted struct is a valid `serverinfo` property.

By default, the expansion looks at the default server in the current working directory. To grab a server property by server name, use this syntax:

```javascript
${serverinfo.serverHomeDirectory@serverName}
${serverinfo.serverHomeDirectory@serverName:defaultValue}
```

## Other properties in the same JSON file

You can self-reference other properties in the same JSON file using the `@` namespace. So given the following JSON file:

```javascript
{
    "appFileGlobs" : "models/**/*.cfc,tests/specs/**/*.cfc",
    "scripts":{
        "format":"cfformat run ${@appFileGlobs} --overwrite",
        "format:check":"cfformat check ${@appFileGlobs} --verbose"
    }
}
```

The expansion of `${@appFileGlobs}` self-references the `appFileGlobs` property inside the same file, allowing for easy re-use of that value.

## Custom namespaces

Modules can register an **onSystemSettingExpansion** interceptor to contribute custom system setting namespace expansions. The interceptor gets the following data in **interceptData**

* **setting** - The name of the setting to be expanded (with ${} and :defaultValue removed)
* **defaultValue** - The default value, or empty string if none specified
* **resolved** - A boolean that should be set true if the interceptor was able to resolve the setting
* **context** - A struct of the original JSON being expanded or the parameters from the command line where the expansion was used.

A hypothetical example would be:

```javascript
function onSystemSettingExpansion( struct interceptData ) {	
  // ${luceeInfo.property}
  if( interceptData.setting.lcase().startsWith( 'luceeinfo.' ) ) {
		
    var settingName = interceptData.setting.replaceNoCase( 'luceeInfo.', '', 'one' );
				
    interceptData.setting = server.lucee[ settingName ] ?: interceptData.defaultValue;
		
    // Stop processing expansions on this setting
    interceptData.resolved=true;
    return true;
  }	
}
```

And then we would use our hypothetical namespace to reference any Lucee information like so

```javascript
echo ${luceeinfo.version}
```

{% hint style="warning" %}
It's important if you implement your own `onSystemSettingExpansion` interceptor that you check the incoming setting to see if it applies to you. If you process the system setting, you must place the final expanded value back in the `interceptData.setting` struct key, set `interceptData.resolved` to `true` and return `true` from the interception method so the chain stops processing.
{% endhint %}
