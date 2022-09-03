# Semantic Versioning

The main goal of semver is to create a common ground among developers and their projects for describing the status of their releases and help users get the versions they want. Semver describes how a proper version number can be formatted as well as a special syntax for describing ranges of versions. There are many ways to describe a version number and semver is just one of them. We chose semver for CommandBox due to its popularity and usage among other common tools like npm. In fact, we've copied the npm flavor or semver specifically which you can [read about here](https://github.com/npm/node-semver/blob/master/README.md).

## Format

If you only get one thing out of all this, at least understand the basic parts of a version. Everything but the major number is optional so remember a version can be as simple or complex as you like and there are rules for dealing with the missing pieces.

```
major.minor.patch-preReleaseID+build
```

Let's digest that bit by bit:

* **major** - This is the major version number of the software. Increase this number when making major changes or breaking compatibility with previous releases.
* **minor** - This number gets incremented when you add new features and improvements to software that doesn't break compat.  Resets to 0 on a major bump.
* **patch** - This number gets incremented for small bug fixes.  Resets to 0 on major and minor bumps.
* **prerReleaseID** - The presence of this means the version is not stable.  This string can be anything, but is typically "**rc**" for release candidate, "**snapshot**", "**alpha**", "**beta**", etc.  You can have more than one prerelease with "**beta.1**", "**beta.2**", "**beta.3**", etc. &#x20;
* **build** - This is a number that usually comes from your build server and increments every time a commit is made that triggers a build.  It can be used to track a very specific release, but should not be used for semver range comparisons.  It's really just for information. &#x20;

So let's take a look at some valid semantic versions.

```
5
3.2
1.7.8
1.2.3-alpha
5.6+0045
2-beta.3
1.2.3-rc.4+9876
```

When writing a package, we recommend using a full version number just to be as descriptive as possible. You may not wish to have a build number if you don't run an automated build. And of course, don't include a prereleaseID unless the package isn't stable. Set your package's version like so:

```
CommandBox> package version 1.0.0
Set version = 1.0.0
CommandBox> bump --minor
Set version = 1.1.0
CommandBox> bump --major
Set version = 2.0.0
```

## Ranges

Ok, so far we've only talked about exact versions. Semantic versioning also gives a very powerful syntax for addressing a range of versions. This can be very useful when installing or updating if you don't care about the exact package version that you get, but you have a few parameters. One of the main ideas behind semver ranges is that you can quickly and easily upgrade 3rd party libraries to the latest version that still works with your code without breaking anything. Ranges would be used when installing a package with the **install** command

```
CommandBox> install coldbox@4.x
```

Or in your box.json to show what versions of a particular dependency you're willing to accept.

```javascript
{
  "dependencies":{
    "coldbox":"^4.3.0"
  }
}
```

Let's look at the basic formats for version ranges.

### X Ranges

Major, minor, and patch numbers can be replaced with an X (or omitted entirely) meaning you don't care what value you get.

```
# Give me the latest version that starts with 5
5.x
5.x.x

# Give me the latest version that starts with 6.2
6.2.x

# Give me the latest version of the package
x
```

### Greater than/less than Ranges

You can use <, >, <=, and >= operators just like you would expect to request all versions **above**, **below**, or **between** other versions.

```
# Any version greater than 1.0.0 starting with 1.0.1
>1.0.0

# Any version greater than or equal to 1.0.0
>=1.0.0

# Any version less than 3.5.0
<3.5

# Any version between 2.0.0 and 2.5.0 inclusive
>=2.0.0 <=2.5.0
```

### Hyphen Ranges

These are just a variation of greater/less than but easier to type. Hyphen ranges are inclusive of the start and end version.

```
# The exact same as the last example above
2.0.0 - 2.5.0
```

### Tilde Ranges

Now we get a little interesting. These last two types of ranges are designed to allow you to keep updating a package version to get compatible versions, but stopping short of getting a breaking update on accident. You usually see these in a box.json file to control what happens when the **update** command is run. A tilde in front of the version will allow patch upgrades when a minor version is specified and will allow minor upgrades when only a major version is specified.

```
# Allow me to upgrade to 1.2.4 or 1.2.5 but not 1.3.0 (same as 1.2.x)
~1.2.3

# Same as above.  Patch can change, but not minor. (same as 3.6.x)
~3.6

# Allow me to upgrade to 4.1, 4.2, or 4.7, but not 5.0 (same as 4.x)
~4
```

### Caret Ranges

This is the default ranged used by CommandBox in your box.json when you install packages from ForgeBox which matches npm's default behavior. A caret allows changes that do not modify the left-most non-zero digit. In other words, this allows patch and minor updates for versions 1.0.0 and above, patch updates for versions 0.1.0, and no updates for versions 0.0.X. The reason for the left-most non-zero digit caveat is for authors who have a whole series of 0.x version releases before ever making it to a full major release. It allows us to essentially treat the minor version as the major version until a "real" major version exists.

```
# Allows 1.2.4 and 1.3, but not 2.0 (same as 1.x.x)
^1.2.3

# Allows 0.2.4 and 0.2.5 but not 0.3 (same as 0.2.x)
^0.2.3
```

### Logical OR

And one final trick. You can string together any of the ranges we showed above with two pipe characters ( || ) and each range will be evaluated until a matching one is found.

```
# Any one of these exact versions
1.0.0 || 2.0.0 || 3.0.0

# Any version starting with 1, greater than or equal to 2.5.0, or between 5.0.0 and 7.2.3 inclusive
1.x || >=2.5.0 || 5.0.0 - 7.2.3
```

If you want more reading or examples, check out the [home page for semver](http://semver.org/) as well as the docs for the [npm semver library](https://github.com/npm/node-semver/blob/master/README.md). You can also peruse the [unit test suite](https://github.com/Ortus-Solutions/commandbox/blob/development/tests/cfml/system/util/TestSemanticVersion.cfc) I have for the CommandBox semver library here which tests every possible combination.
