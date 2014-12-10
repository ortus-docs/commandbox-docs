# Installing Packages

The install command is used to tell CommandBox what you want. Here we ask for the stable release of the ColdBox MVC Platform. "coldbox" is the name of the ForgeBox slug.

```
install coldbox
```

## Install Process

When you install a package, here are the steps that are taken.  Most all of this should be evident by the output streamed to the console during the install process.  To get even more juicy details, use the --verbose flag while installing.

1. CommandBox checks Forgebox to verify the slug
2. The local artifact cache is checked for the package
3. If not found, the package is downloaded and stored as an artifact
4. The package is unzipped and its box.json is read
5. Installation directory is finalized
6. Contents of package are copied based the ignoreList and --production flag
7. The package is saved as a dependency in the root box.json
8. The package's dependencies are installed

## Installation Directory

There are several factors that can determine where a package gets installed to.  Here are the ways CommandBox determines the install location in order of importance.

1. The value of the **directory** parameter passed into the install command by the user.
2. The value of the **installPaths.packageName** propert set in the project's main box.json by the user.
3. The value of the **directory** property in the package's box.json by the package author. Note, this must be a path relative to the current working directory (CWD).
4. Based on the package type convention if the package is a command, module, plugin, or interceptor.
5. The current working directory (CWD)

Once the installation directory is determined, a folder is created that matches the package's slug which is where the package is finally copied to.  If the package's createPackageDirectory property is set to false in the box.json, the package will be copied to the root of the installation directory.  An example of this would be a complete application that needs to go in the web root.

## Saving dependencies

You can install a package as a **dependency** or a **development dependency**.  Regular dependencies are ones required for operation of the main package.  Development dependencies are optional and only necessary if you plan on making changes to the package you're installing.  Dev dependencies would include testing frameworks or build tools.

Packages you 



