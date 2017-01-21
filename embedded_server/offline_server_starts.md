# Offline Server Starts

Sometimes you may wish to start a server on a computer that doesn't have access to the Internet.  However, you may notice that running a command such as the following will throw an error trying to connect to ForgeBox:

```bash
start cfengine=lucee@5.x
```

That is because `5.x` is a semver range and not a specific version.  CommandBox must connect to ForgeBox to see what versions of the CF engine `lucee` can be found to use the latest one.

## Be More Specific

If you know that a CF engine is already downloaded in your server's artifacts directory, start your server with a specific major, minor, and patch version to skip the ForgeBox check.

```bash
start cfengine=lucee@5.0.0+252
```

NOTE: If this fails, check the location of the folder structure below the artifacts folder for it's exact path.  
e.g. /artifacts/lucee/5.0.0+252/lucee.zip - The box command for this would be: start cfengine=lucee@5.0.0+252  
This follows the pattern:  
/artifacts/\[server-type\]/\[server-version\]/\[server-type\].zip  
Useful if you've manually downloaded the file from forgebox and need to rename it.

## Fail Safe

If CommandBox needs to connect to ForgeBox to resolve a version number and ForgeBox is unavailable, it will look in your local artifacts cache to try and find a cached version of that CF engine that satisfies your semver range.  This means you may not get the latest version of that CF Engine, but at least your server will start up.

