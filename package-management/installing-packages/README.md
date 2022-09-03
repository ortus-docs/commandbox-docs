# Installing Packages

The install command is used to tell CommandBox what you want. Here we ask for the stable release of the ColdBox MVC Platform. "coldbox" is the name of the ForgeBox slug.

```bash
install coldbox
```

Packages should always have a `box.json` descriptor file inside them. This is especially true of packages installed from endpoints other than Forgebox since they don't have any other metadata available. CommandBox will install any zip file even if it doesn't have a `box.json`, but this isn't ideal since the name, version, and type of the package must be guessed in that instance.

If you find a package on the internet that doesn't have a `box.json`, please contact the maintainer and request that they add one or submit a pull request!

## ForgeBox Semantic Versioning Support

ForgeBox supports semantic version ranges for installing packages. Here are some examples:

```bash
# Latest stable version
CommandBox> install foo

# Same as above
CommandBox> install foo@stable

# latest version, even if pre release (bleeding edge)
CommandBox> install foo@be

# A specific version
CommandBox> install foo@1.2.3

# Any version with a major number of 4 (4.1, 4.2, 4.9, etc)
CommandBox> install foo@4.x

# Any version greater than 1.5.0
CommandBox> install foo@>1.5.0

# Any version greater than 5.2 but less than or equal to 6.3.4
CommandBox> install "foo@>5.2 <=6.3.4"

# Any version greater than or equal to 1.2 but less than or equal to 3.2
CommandBox> install "foo@1.2 - 3.2"

# Allows patch-level changes if a minor version is specified. Allows minor-level changes if not.  (2.1.2, 2.1.3, 2.1.4, etc)
CommandBox> install foo@~2.1

# Any greater version that does not modify the left-most non-zero digit.  4.2, 4.3, 4.9, etc
CommandBox> install foo@^4.1.4
```

## Install Process

When you install a package, here are the steps that are taken. Most all of this should be evident by the output streamed to the console during the install process. To get even more juicy details, use the `--verbose` flag while installing.

1. CommandBox inspects the ID passed to the `install` command to determine the endpoint to use.
2. The matching endpoint is asked to fetch the package represented by the ID.&#x20;
3. For example, the ForgeBox endpoint checks the local artifact cache and possibly downloads the package.
4. If ForgeBox is offline, the best match package will be looked for in your artifacts.
5. The package is unzipped and its `box.json` is read
6. Installation directory is finalized
7. Contents of package are copied based on the `ignore` and `--production` flag
8. The package is saved as a dependency in the root box.json
9. The package's dependencies are installed
