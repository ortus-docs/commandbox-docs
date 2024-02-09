# Installation Path

There are several factors that determine where a package gets installed to. Here are the ways CommandBox determines the install location in order of importance.

1. The value of the `directory` parameter passed into the `install` command by the user.
2. The value of the `installPaths.packageName` property set in the project's main box.json by the user. Where `packageName` is the name of the package you are installing like `logbox`.
3. The value of the `directory` property in the package's box.json by the package author. Note, this must be a path relative to the current working directory (CWD).
4. Based on the package type convention if the package is a command, coldbox module, commandbox module, plugin, or interceptor. (These default conventions can be overriden on a per-package basis)
5. If the package being installed is of type `lucee-extensions` and if the current working directory is found to have a Lucee server in it, the lex file will instead be installed to the server context's deploy folder.
6. The current working directory (CWD)

Once the installation directory is determined, a folder is created that matches the package's slug which is where the package is finally copied to. If the package's createPackageDirectory property is set to false in the box.json, the package will be copied to the root of the installation directory. An example of this would be a complete application that needs to go in the web root.

### Override package install paths by convention

If you have a project and want all packages of a certain type to use a different-than-normal default install location, you can override each package type just for that project.  Create an `installPathConventions` key in the containing package's `box.json` which is an object containing keys for each package type you wish to override package install paths for.

```json
{
  "installPathConventions" : {
    "modules" : "../lib/modules",
    "mvc" : "../lib/framework",
    "testing" : "they-will-never-find-it-here-lol/"
  }
}
```

These can be absolute paths, or relative (to the `box.json`).  The `install` command will check for these when it installs a package.  These overrides will ONLY apply to the first level of dependencies.  For example, if you set up a `modules` override to put your app's modules into `../lib/modules`, any downstream dependencies of those modules will still use the default install location, unless those modules have their OWN `installpathConventions` defined.
