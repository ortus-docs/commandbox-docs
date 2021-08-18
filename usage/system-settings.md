# System Settings

Now that we're starting to use CommandBox in a lot of cloud scenarios like Docker, we're looking for more and more ways to have dynamic configuration. The most common way to do this is via Java system properties and environment variables. We've wrapped up those two into a new concept called **System Settings**. Now any time you use `${mySetting}` in a command parameter, a `box.json` property, `server.json` property, or a **Config Setting**, that place holder will be replaced with a matching JVM property or env var \(in that order\) at runtime. This is great for setting things like ports, default directories, or passwords and other secrets as an env variable so it can be different per server and not part of your code.

## Using system settings from the CLI

You can test it out easily by outputting your system path like so:

```text
echo ${PATH}
```

## Default values

If a system setting can't be found in a Java property or an environment variable, an empty string will be returned. You can provide a default value like so.

```text
server start port=${SERVER_PORT:8080}
```

This does assume that your default value will never contain a colon!

## Lookup Order

System settings are looked up in the following order. If the same variable exists in more than one place, the first one found will be used:

1. Environment variables for the currently executing command
2. Environment variables for the parent \(calling\) command \(if applicable\)
3. Global shell environment variables
4. JVM System Properties in the CLI process
5. Environment Variables from your actual operating system

For example, if you run the following it will output the contents of your OS's `PATH` environment variable.

```text
echo ${path}
```

However, if you set a shell environment variable from inside CommandBox called `path` and then output it, you will see the contents of your variable since it overrides.

```text
set path=donuts
echo ${path}
```

## Using system settings in JSON files

When `box.json` or `server.json` files are read, they automatically have all system setting setting placeholders swapped out. For instance, you can specify the port for your server in your `server.json` like so:

```text
 server set web.http.port=\${WEB_PORT:8080}
```

Note, we escaped the system setting by putting a backslash \(`\`\) in front of it. That's because we wanted to insert the actual text into the file and not the value of it! The resultant `server.json` is this. Note the system setting needs to be encased in quotes so it's just a string for the JSON.

```javascript
{
    "web":{
        "http":{
            "port":"${WEB_PORT:8080}"
        }
    }
}
```

Now, if your server has an environment variable called `WEB_PORT`, it will be used as the port for your server.

System settings can also be used in object key names as well in your JSON files. Here is an example of a `.cfconfig.json` file with a dynamic datasource name.

```javascript
{
  "datasources":{
    "myDSN-${environment}":{
      "database":"test",
      "dbdriver":"MSSQL",
      "dsn":"jdbc:sqlserver://{host}:{port}",
      "host":"localhost",
      "password":"password",
      "username":"user"
    }
  }
}
```

Note if there are duplicate key names after the system settings are expanded, the last one expanded will win.

## In the REPL

You can use system settings and environment variables in the REPL using the same syntax as the CLI

```javascript
CommandBox> set foo=bar
CommandBox> REPL

CFSCRIPT-REPL: echo( '${foo}' )
bar
```

## Manual system setting replacements

If you're writing a custom command or task runner that reads a JSON file of your own making, you can do easy system setting replacements on the file.

## In a complex data structure

```javascript
component {
    property name='systemSettings' inject='SystemSettings';

     function run() {
        var mySettings = deserializeJSON( fileRead( 'mySpecialConfigFile.json' ) );
        systemSettings.expandDeepSystemSettings( mySettings );
     }

}
```

## In a string

The `expandDeepSystemSettings()` method will recursively crawl the struct and find any strings with system setting placeholders inside them. Be careful not to write back out the same struct after you've done replacements on it. Otherwise, you'll overwrite the placeholders with the current values!

You can also manually replace system setting placeholders in a single string like so:

```javascript
var myValue = 'User home is in ${user.home}';
myValue = systemSettings.expandSystemSettings( myValue );
```

## Programmatic access

The `SystemSettings` service also gives you programmatic access to individual system settings in your custom commands and task runners.

```javascript
var mySetting = systemSettings.getSystemSetting( 'settingName' );
or
var mySetting = systemSettings.getSystemSetting( 'settingName', 'defaultValue' );
```

