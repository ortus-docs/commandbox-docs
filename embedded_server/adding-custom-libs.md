# Adding Custom Libs

If you have custom jar files that you want to be available to a server, we recommend you use the \`this.javaSettings\` functionality in your \`Application.cfc\` to load those jars.  If that isn't an option for you, or you want to include libs for a JDBC drivers then we offer a feature for you to specify a list of directories for which CommandBox will load jars from.  This prevents you from worrying about getting your jars inside the \`WEB-INF/lib\` which starts fresh anytime you forget a server.

To load in your jar files, the first method is to use the \`libDirs\` parameter to the \`server start\` command.

```
server start libDirs=path/to/libs,another/path/to/libs
```

And the way to specify this in a portable manner in your \`server.json\` is like so:

```
server set app.libDirs=path/to/libs,another/path/to/libs
server start
```

Remember, paths to the \`start\` command are relative to your current working directory and paths in your \`server.json\` file are relative to the folder that the \`server.json\` file lives in.  Absolute paths work the same everywhere.

## Global Custom Libs

Have a bunch of servers and they ALL need the same jars, you can add your \`libDirs\` to the global server defaults config settings.  Your global lib directories won't be overwritten by server-level config dirs, but instead appended to.  Relative paths in your config settings are relative to the respective web root of the server.  CommandBox will also ignore missing lib dirs to give you some flexibility in this.

```
config set servers.defaults.app.libDirs=/path/to/global/libs
```





