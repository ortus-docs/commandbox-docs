# Gist

CommandBox can install a Github Gist from [gist.github.com](https://gist.github.com/) as a package.

Make sure the root of your Git repo has a `box.json` inside of it so CommandBox can tell the version and name of the package. If there is no `box.json`, the name of the Gist ID will be used as the package name.

## Installation

To install a package from a Github Gist, you must pass the Gist ID from gist.github.com:

```bash
install gist:<gistID>
install gist:b6cfe92a08c742bab78dd15fc2c1b2bb
```

The Github username is optional.

```bash
install gist:<username>/<gistID>
```

You can target a specific `commit` by adding a "commit-ish" after the Gist ID.

```bash
install gist:b6cfe92a08c742bab78dd15fc2c1b2bb#37348a126f1f410120785be0d84ad7a2148c3e9f
```

## In box.json

You can specify packages from folder endpoints as dependencies in your `box.json` in this format. Remember, JSON requires that backslashes be escaped.

```javascript
{
    "dependencies" : {
        "myPackage" : "gist:gistID"
    }
}
```

