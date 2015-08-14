# HTTP(S) Endpoint

Packages hosted on a website as a zip file can be installed by using the direct URL to the package.  Both HTTP and HTTPS URLs are supported.  If the URL returns a `301` or `302` redirect, it will be followed until the package is reached.

Make sure your package zip file has a `box.json` inside of it so CommandBox can tell the version and name of the package.  If there is no `box.json`, the following rules will be decided to determine the name of the package:

1. If the URL has the zip file name in it, the name without ".zip" is used.
2. If the URL contains `github.com`, the repo name will be used.
3. Otherwise, the entire URL will have non alpha-numeric characters removed and used.

## Installation

To install a package from a website, use the full URL like so:

```bash
install http://www.site.com/myPackage.zip
```

## In box.json

You can specify packages from HTTP(S) endpoints as dependencies in your `box.json` in this format:

```javascript
{
    "dependencies" : {
        "myPackage" : "http://www.site.com/myPackage.zip"
    }
}

```
