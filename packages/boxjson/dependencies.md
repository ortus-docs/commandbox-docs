# Dependencies

## dependencies

**object**

This object tracks the other packages that are required to run the project.  Packages are added here automatically by the `install slug` command, but you can also manually add them and just type `install` to install them.

The key is the unique slug of the package and the value is the a semvar range, local directory, URL to a zip, or a source control URL.

```javascript
"Dependencies" : {
    "coldbox" : "x" // latest version from ForgeBox
    "cborm" : "1.0.1", // a specific version from ForgeBox
    "private-package" : "C:\libs\foo", // local filepath
    "baz" : "http://site.com/baz.zip", // URL to zip file
    "cbcsrf" : "https://github.com/ColdBox/cbox-csrf.git"  // Git/svn endpoint
}
```



*Currently, the slug is the only data used in the `dependencies` object.  Versions and other code endpoints will be implemented soon in future version.*

## devDependencies

**object**
