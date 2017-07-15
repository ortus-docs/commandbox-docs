# System Settings (Environment Variables and Java Properties)

Now that we're starting to use CommandBox in a lot of cloud scenarios like Docker, we're looking for more and more ways to have dynamic configuration.  The most common way to do this is via Java system properties and environment variables.  We've wrapped up those two into a new concept called **System Settings**.  Now any time you use `${mySetting}` in a command parameter, a `box.json` property, `server.json` property, or a **Config Setting**, that place holder will be replaced with a matching JVM property or env var (in that order) at runtime.  This is great for setting things like ports, default directories, or passwords and other secrets as an env variable so it can be different per server and not part of your code.  

## Using system settings from the CLI

You can test it out easily by outputting your system path like so:

```
echo ${PATH}
```

## Default values 

If a system setting can't be found in a Java property or an environment variable, an empty string will be returned.  You can provide a default value like so.

```
server start port=${SERVER_PORT:8080}
````

This does assume that your default value will never contain a colon!

## Using system settings in JSON files

When `box.json` or `server.json` files are read, they automatically have all system setting setting placeholders swapped out.  For instance, you can specify the port for your server in your `server.json` like so:

```
 server set web.http.port=\${WEB_PORT:8080}
```
Note, we escaped the system setting by putting a backslash (`\`) in front of it.  That's because we wanted to insert the actual text into the file and not the value of it!  The resultant `server.json` is this.  Note the system setting needs to be encased in quotes so it's just a string for the JSON.

```json
{
    "http":{
            "port":"${WEB_PORT:8080}"
        }
    }
}
```
Now, if your server has an environment variable called `WEB_PORT`, it will be used as the port for your server.


## Manual system setting replacements

If you're writing a custom command or task runner that reads a JSON file of your own making, you can do easy system setting replacements on the file like this.

```js
component {
    property name='systemSettings' inject='SystemSettings';
    
     function run() {
        var mySettings = deserializeJSON( fileRead( 'mySpecialConfigFile.json' ) );
        systemSettings.expandDeepSystemSettings( mySettings );
     }
    
}
```