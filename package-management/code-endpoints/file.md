# File

Packages that are either stored locally on your machine or are accessible via a network drive as a zip file can be installed by using their file system path. The path can be absolute or relative.

Make sure your package zip file has a `box.json` inside of it so CommandBox can tell the version and name of the package. If there is no `box.json`, the name of the file without the extension will be used as the package name.

## Installation

To install a package from a local file, use the path like so:

```bash
install /var/libs/myPackage.zip
```

Note if using Windows, you need to escape backslashes in the command parameter.

```bash
install C:\\websites\libs\\myPackage.zip
```

Relative paths will start in the directory where the command is being run from.

```bash
install libs/myPackage.zip
install ../../libs/myPackage2.zip
```

## In box.json

You can specify packages from file endpoints as dependencies in your `box.json` in this format. Remember, JSON requires that backslashes be escaped.

```javascript
{
    "dependencies" : {
        "myPackage" : "/var/libs/myPackage.zip"
        "myPackage2" : "C:\\websites\libs\\myPackage2.zip"
    }
}
```
