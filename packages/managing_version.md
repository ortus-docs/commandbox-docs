# Managing Package Version

One of the more important pieces of information for a package is the version.  We encourage people to use semantic versioning for their packages.  The spec for this can be read here:

[http://semver.org](http://semver.org)


## Semantic Versioning

The spec involves a lot of possible variations, but to simplify, the basics involve having a three-part version number.   

```
<major>.<minor>.<patch>
```
And constructed with the following guidelines:

* Breaking backward compatibility or major features bumps the major (and resets the minor and patch)
* New additions without breaking backward compatibility bumps the minor (and resets the patch)
* Bug fixes and misc changes bumps the patch

An example would be `1.0.0`

## Setting Your Version

When you initialize a folder as a package with the `init` command, the version will be set to `0.0.0` by default.  You can use `init --wizard` to be asked what version you'd like to use, among other questions.

The version is stored in your package's `box.json` and you can check it at any time by running

```bash
package version
```

To override the version at any time, pass the new version into the `package version` command like so:

```bash
package version 2.3.7
```

## Bumping The Version

As a handy shortcut, you can have CommandBox automatically bump the version for you after making changes to your package.

### Patch Bump

If you just fixed a small bug or UI change in your package, run this to bump the minor patch.  For example, this would change `2.3.7` to be `2.3.8`.

```bash
bump --patch
```

### Minor Bump
Let's say you've added a few nice features or enhancements to your project but it's still backwards compatible with previous versions.  Run this to bump the minor version.  Note this will reset your patch version to `0`.  For example, this would change `2.3.8` to be `2.4.0`.

```bash
bump --minor
```

### Major Bump
Now, you really got busy over the weekend and made a major overhaul to your project-- specifically introducing some changes that introduce backwards compatibilty.  Let's say you've added a few nice features or enhancements to your project but it's still backwards compatible with previous versions.  Run this to bump the minor version.  Note this will reset your patch version to `0`.  For example, this would change `2.3.8` to be `2.4.0`.

```bash
bump --minor
```

