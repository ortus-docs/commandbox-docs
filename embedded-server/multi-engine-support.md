# Multi-Engine Support

You can specify the CFML engine via the command line arguments:

```bash
CommandBox> start cfengine=adobe@9
```

This will start an Adobe ColdFusion 9 server in your webroot. That's it!

By default, CommandBox uses the `cfengine` slug to search for the engine on ForgeBox. The format is `slug@version` where the version is optional. Ortus Solutions maintains the versions of the engines available on ForgeBox.

Supported engines are:

* Adobe ColdFusion 9 \*\*
* Adobe ColdFusion 10
* Adobe ColdFusion 11
* Adobe ColdFusion 2016
* Adobe ColdFusion 2018
* Railo 4.2
* Lucee 4.5
* Lucee 5

Here are some examples:

```bash
# Start the default engine
CommandBox> start

# Start the latest stable Railo engine
CommandBox> start cfengine=railo

# Start a specific engine and version
CommandBox> start cfengine=adobe@10.0.12

# Start the most recent Adobe server that starts with version "11"
CommandBox> start cfengine=adobe@11

# Start the most recent adobe engine that matches the range
CommandBox> start cfengine="adobe@>9.0 <=11"
```

Engines are downloaded and stored in your CommandBox artifacts folder. You can view your engines and clear them using the standard artifacts commands:

```bash
CommandBox> artifacts list

# Removes all adobe servers currently in the artifacts
# These servers will need to be re-downloaded the next time they are started
CommandBox> artifacts remove adobe
```

> \*\* **Note**: [Adobe ColdFusion 9 does not support Java 8](http://blogs.coldfusion.com/post.cfm/which-jdk-is-supported-with-coldfusion-9-10-and-11). To run ColdFusion 9 you must first [install Java 6 or 7](https://www.java.com/en/download/faq/other_jreversions.xml). Then change the CommandBox java version, by doing one of the following:
>
> * Update the OS default, defined in the `JAVA_HOME` environment variable, with the Java 6/7 JRE directory
> * Copy the Java 6/7 JRE folder into the CommandBox installation directory, and rename the subfolder "jre".
>
>   For example, if the new JRE is installed in `C:\Program files\java\jre7` copy it to `C:\CommandBox\jre`
>
>   _Note: This version will take precedence over_ `JAVA_HOME`
>
> Restart CommandBox to apply the changes.

## ColdFusion Admin settings

While Lucee asks for a password the first time running the admin, ColdFusion requires a username and password when CommandBox sets it up. The default username and password for the Adobe ColdFusion servers used are:

* Username: `admin`
* Password: `commandbox`

## WAR Support

Additionally, CommandBox can start any WAR given to it using the `WARPath` argument.

```bash
CommandBox> start WARPath=/var/www/myExplodedWAR
CommandBox> start WARPath=/var/www/myWAR.war
```

If you run a regular `start` command inside of a folder that has a `/WEB-INF/web.xml` file, CommandBox will treat that folder as a WAR.

## Custom Engines

The `cfengine` parameter can accept any valid CommandBox endpoint ID. That means it can be an HTTP URL, a Git repo, a local folder path to your company's network share, or a custom ForgeBox entry you've created. As long as that endpoint resolves to a package that contains these files, you're good:

1. `box.json`
2. `Engine.[zip|war]` \(file name doesn't matter\)

CommandBox will download the package, unzip it and use the WAR/zip file as the engine for your app.

Normally, the artifacts cache isn't used for non-ForgeBox packages, but CommandBox will only download the engine once per server and then assume the file hasn't changed. You will need to forget the server to trigger a new download.

Here's an example of starting up a web server using a direct download link to a package containing a WAR file:

```bash
CommandBox> start cfengine=http://downloads.ortussolutions.com/adobe/coldfusion/9.0.2/cf-engine-9.0.2.zip
```

## server.json Configuration

You can set the `cfengine` and other related configuration options in your `server.json` to use them every time you start your app.

```bash
CommandBox> server set app.cfengine=adobe
CommandBox> server set app.WARPath=/var/www/my-app
```

These commands would create the following `server.json`

```javascript
{
    "app":{
        "cfengine":"adobe",
        "WARPath":"/var/www/my-app"
    }
}
```

Just a reminder that starting a server with any command line arguments will save the arguments to your `server.json` by default.

```bash
CommandBox> start cfengine=adobe@9
```

This command would add `adobe@9` to your `server.json`. If this is not what you want, you can append `saveSettings=false` or even `--!saveSettings` when you start your server and CommandBox will not save the arguments you specify to your `server.json`.

