# Installing Packages

The install command is used to tell CommandBox what you want. Here we ask for the stable release of the ColdBox MVC Platform. "coldbox" is the name of the ForgeBox slug.

```bash
install coldbox
```

## Install Process

When you install a package, here are the steps that are taken.  Most all of this should be evident by the output streamed to the console during the install process.  To get even more juicy details, use the `--verbose` flag while installing.

1. CommandBox checks Forgebox to verify the slug
2. The local artifact cache is checked for the package
3. If not found, the package is downloaded and stored as an artifact
4. The package is unzipped and its box.json is read
5. Installation directory is finalized
6. Contents of package are copied based the ignoreList and --production flag
7. The package is saved as a dependency in the root box.json
8. The package's dependencies are installed

