# Installation

These properties affect how and where the package is installed.  

## directory

**string**

The directory, relative to the web root, that the package will be installed to.  This will override the convention directory based on the `type` property.  See package installation for more details on where packages install to.

## type

**string**

The ForgeBox type of the package. See list of available types with the `forgebox types` command.  This can determine the directory the package is installed to.  For instance, a type of `modules` goes in the site's `/modules` directory.

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


## installPaths

**object**
## createPackageDirectory

**boolean**
## packageDirectory

**string**