# File Endpoint

Packages that are either stored locally on your machine or are accessible via a network drive as a zip file can be installed by using their file system path.  The path can be absolute or relative.

Make sure your package zip file has a `box.json` inside of it so CommandBox can tell the version and name of the package.  If there is no `box.json`, the name of the file without the extension will be used as the package name.

## Installation

To install a package from a local file, use the path like so:

```bash
install /var/libs/myPackage.zip
```

Note if using Windows, you need to escape backslashes in the command parameter.

```bash
install C:\\websites\libs\\myPackage.zip
```

## In box.json

You can specify packages from ForgeBox as dependencies in your `box.json` in this format:

```javascript
{
    "dependencies" : {
        "myPackage" : "http://www.site.com/myPackage.zip"
    }
}

```
