# Installing Packages

The install command is used to tell CommandBox what you want. Here we ask for the stable release of the ColdBox MVC Platform. "coldbox" is the name of the ForgeBox slug.

```bash
install coldbox
```

Packages should always have a `box.json` descriptor file inside them.  This is especially true of packages installed from endpoints other than Forgebox since they don't have any other metadata available.  CommandBox will install any zip file even if it doesn't have a `box.json`, but this isn't ideal since the name, version, and type of the package must be guessed in that instance.

## Install Process

When you install a package, here are the steps that are taken.  Most all of this should be evident by the output streamed to the console during the install process.  To get even more juicy details, use the `--verbose` flag while installing.

1. CommandBox inspects the ID passed to the `install` command to determine the endpoint to use.
2. The matching endpoint is asked to fetch the package represented by the ID. 
3. For example, the ForgeBox endpoint checks the local artifact cache and possibly downloads the package.
4. The package is unzipped and its box.json is read
5. Installation directory is finalized
6. Contents of package are copied based the ignoreList and --production flag
7. The package is saved as a dependency in the root box.json
8. The package's dependencies are installed

