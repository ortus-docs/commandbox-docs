# Installation

These properties affect how and where the package is installed.  

## directory

**string**

The directory, relative to the web root, that the package will be installed to.  This will override the convention directory based on the `type` property.  See package installation for more details on where packages install to.

```bash
package set directory=lib
package show directory
```

## type

**string**

The ForgeBox type of the package. See list of available types with the `forgebox types` command.  This can determine the directory the package is installed to.  For instance, a type of `modules` goes in the site's `/modules` directory.

```bash
package set type=modules
package show type
```

## ignore

**array**

An array of file/folder globbing patterns that follow the .gitIgnore syntax to NOT be copied when doing a `--production` install.

End a pattern with a slash to only match a directory. Start a pattern with a slash to start in the root.
* `foo` will match any file or folder in the directory tree
* `/foo` will only match a file or folder in the root
* `foo/` will only match a directory anywhere in the directory tree
* `/foo/` will only match a folder in the root

Use a single `*` to match zero or more characters INSIDE a file or folder name (won't match a slash).
* `foo*` will match any file or folder starting with `foo`
* `foo*.txt` will match any file or folder starting with `foo` and ending with `.txt`
* `*foo` will match any file or folder ending with `foo`
* `a/*/z` will match `a/b/z` but not `a/b/c/z`
 
Use a double `**` to match zero or more characters including slashes. This allows a pattern to span directories.
* `a/**/z` will match `a/z` and `a/b/z` and `a/b/c/z`

```bash
package set ignore="['**/.*','test','tests']" --append
package show ignore
```


## installPaths

**object**

This is an object of string values where each key is the slug of an installed package and the value is the path the package is installed to.  In most cases, this will be managed automatically by the `install` and `uninstall` command.  If you want to override the `directory` property on one of your dependencies, you can configured an install path prior to installing the package and it will be used.

Install paths can be a directory relative to the web root (no leading slash) or a full path starting with a drive root.

```javascript
"installPaths" : {
    "coldbox" : "coldbox" // relative to package root (no leading slash)
    "feeds" : "modules/feeds", // relative to package root (no leading slash)
    "Name" : "C:\foo\bar" // Outside root, so full path
    }
```

```bash
package set installPaths=""
package show installPaths
```

## createPackageDirectory

**boolean**

By default when a package is installed, a directory is created in the install path that is named after the package slug.  Setting `createPackageDirectory` to `false` will skip the creation of that folder and dump the contents of the package right into the install path.  

An example of this would be a full application that *is* the entire web root.  Another example would be an interceptor that gets put directly in the `interceptors` folder.

Note, when this is set to `false`, no path will be added to the `installpaths` directory and the package cannot be removed by the `uninstall` command.

```bash
package set 
package show 
```

## packageDirectory

**string**

By default when a package is installed, a directory is created in the install path that is named after the package slug.  If a `packageDirectory` property is set, the folder is named after it instead of the slug. 

An example would be the `coldbox-be` slug that still needs to install into a folder called `coldbox`.  You shouldn't need to use this setting.

```bash
package set 
package show 
```