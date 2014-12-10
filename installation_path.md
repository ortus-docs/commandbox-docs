# Installation Path

There are several factors that can determine where a package gets installed to.  Here are the ways CommandBox determines the install location in order of importance.

1. The value of the **directory** parameter passed into the install command by the user.
2. The value of the **installPaths.packageName** propert set in the project's main box.json by the user.
3. The value of the **directory** property in the package's box.json by the package author. Note, this must be a path relative to the current working directory (CWD).
4. Based on the package type convention if the package is a command, module, plugin, or interceptor.
5. The current working directory (CWD)

Once the installation directory is determined, a folder is created that matches the package's slug which is where the package is finally copied to.  If the package's createPackageDirectory property is set to false in the box.json, the package will be copied to the root of the installation directory.  An example of this would be a complete application that needs to go in the web root.

