# Dependencies

## dependencies

**object**

This object tracks the other packages that are required to run the project. Packages are added here automatically by the `install ID` command, but you can also manually add them and just type `install` to install them.

The key is the unique slug of the package and the value is the a semvar range, local directory, URL to a zip, or a source control URL.

```javascript
"dependencies" : {
    "coldbox" : "x" // latest version from ForgeBox
    "cborm" : "1.0.1", // a specific version from ForgeBox
    "private-package" : "C:\\libs\\foo", // local folder
    "private-package2" : "C:\\libs\\foo.zip", // local file
    "baz" : "http://site.com/baz.zip", // URL to zip file
    "cbcsrf" : "git://github.com/ColdBox/cbox-csrf.git"  // Git endpoint
}
```

```bash
package set dependencies="{ coldbox : '4.0.0' }" --append
package show dependencies
```

## devDependencies

**object**

Development dependencies operate the same as regular ones, expect they are not required to use this project. Instead, they are only required if you plan to do development on the project. These usually including testing libraries, etc.

The `install` command will save dependencies here when you use the `--saveDev` flag. These packages will be skipped when you run `install --production`

```javascript
"devDependencies" : {
    "testbox" : "2.0"
}
```

```bash
package set devDependencies="{ cbdebugger : '1.0.0' }" --append
package show devDependencies
```

