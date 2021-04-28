# Server Home

When you start a CF server in CommandBox, the default behavior is that the WAR for that version is expanded into a folder inside the `servers` directory inside your CommandBox home.  There is a sub folder containing an MD5 hash of the server name and web root concatenated with the server name.  Inside of that, is a folder containing the name of the CF Engine followed by the engine version.  The result is something like this:

```bash
~/.CommandBox/server/C3D9460AC18B18AC462BBBF1E02425C8-cfconfig/lucee-5.3.7.48
```

When you start a new version of the CF engine, the old folder is left in place, and a new one is created containing the new WAR.  e.g.

```bash
~/.CommandBox/server/C3D9460AC18B18AC462BBBF1E02425C8-cfconfig/lucee-5.3.8-RC+139
```

This allows you to easily revert back to the previous version as CommandBox automatically handles the server homes for you.  Running `server forget` will remove the entire top level folder containing all the engine versions. 

### Configuration management on upgrade

When you start a new server version to replace a previous version, you will get a new server home which also means all settings and manual changes to the old server home will not exist in the new one.  if you have `commandbox-cfconfig` installed, it will automatically copy over all CF config settings that it can handle into the new server home.  Any other custom changes you've made such as license files, logs, jars or `web.xml` modifications will not be brought over.

## Custom Server Home Directory

You can move the server home to a custom folder of your choice with the following setting:

```bash
server set app.serverHomeDirectory=/absolute/path/to/serverHome
# Or...
server set app.serverHomeDirectory=relative/serverHome
```

Note, the relative path is relative to the folder that the `server.json` lives in.  This can give you a predictable server home, but comes with a few caveats:

* CommandBox will NOT auto-upgrade new versions of the CF engine
* If you ask for a new version of the engine, CommandBox will ignore it
* You must `server forget` and remove the server home before you can start a new server in your custom server home.

## Single Server Home

There is an option that will give you a mix of a custom server home and the default behavior where the server home is automatically placed inside the CommandBox home.  This is the `app.singleServerHome` setting.  It defaults to `false`.

```bash
server set app.singleServerHome=true
```

This will create a folder similar to the default behavior above, but WITHOUT the version number appended to the folder name.  An example server home would be:

```bash
~/.CommandBox/server/C3D9460AC18B18AC462BBBF1E02425C8-cfconfig/lucee
```

This has the convenience of auto-managing the server home for you, but still come with all the caveats listed above for a custom server home such as requiring you to `server forget` to upgrade or change versions via CommandBox. 

## Single Server Mode

This setting is a CLI-wide config setting that forces CommandBox to only have a single server.  This setting was created to simplify our Ortus Docker Images and you likely DON'T want to use this outside of a scenario such as a Docker image where only one server exists.  When this is enabled, the first server that is started will be remembered as the only serer that exists.  No matter what directory you run `start` and `stop` commands from and no matter what `name` you pass, CommandBox will always perform that action against the single server it has.  You can `server forget` and start another server elsewhere, but there can only be one server at a time.  This setting was added to help prevent duplicate servers from getting created in different folders during Docker builds which mixed up the settings.

```bash
config set server.singleServerMode=true
```

