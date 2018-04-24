# Folder

Packages that are either stored locally on your machine or are accessible via a network drive in an unzipped folder can be installed by using their file system path. The path can be absolute or relative.

Make sure your package folder has a `box.json` inside of it so CommandBox can tell the version and name of the package. If there is no `box.json`, the name of the last folder in the path will be used as the package name.

## Installation

To install a package from a local folder, use the path like so:

```bash
install /var/libs/myPackage/
```

Note if using Windows, you need to escape backslashes in the command parameter.

```bash
install C:\\websites\libs\\myPackage\\
```

Relative paths will start in the directory where the command is being run from.

```bash
install libs/myPackage/
install ../../libs/myPackage2/
```

## In box.json

You can specify packages from folder endpoints as dependencies in your `box.json` in this format. Remember, JSON requires that backslashes be escaped.

```javascript
{
    "dependencies" : {
        "myPackage" : "/var/libs/myPackage/"
        "myPackage2" : "C:\\websites\libs\\myPackage\\"
    }
}
```

