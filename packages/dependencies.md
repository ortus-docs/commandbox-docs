# Dependencies

When a package is installed, CommandBox will read its dependencies (from the box.json) and recursively install them as well. This encourages developers to write small, reusable libraries for everyone to use. When installing via a package manager, you don't have to worry about getting all the pieces installed.

You can install a package as a **dependency** or a **development dependency**.  Regular dependencies are ones required for operation of the main package.  Development dependencies are optional and only necessary if you plan on making changes to the package you're installing.  Dev dependencies would include testing frameworks or build tools. 