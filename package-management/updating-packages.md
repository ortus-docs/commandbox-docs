# Updating Packages

CommandBox does more than help you install packages. It also helps you keep them up to date as well. Remember, you can always get a quick list of all the dependencies installed your app with the `list` command.

```bash
CommandBox> list
Dependency Hierarchy myApp (1.0.0)
├── coldbox (4.2.0)
└─┬ cbvalidation (1.0.0)
  └── cbi18n (1.0.0)
```

## Getting the Latest

To check and see if any of your installed packages can be updated to a newer version, run the `update` command.

![update command](../.gitbook/assets/image.png)

Entering "yes" will install the newest version of the package. Use the --force flag to automatically answer "yes". It is also possible to get a list of outdated dependencies without the prompt to update them with the `outdated` command.

```bash
outdated
```

The table of information in the `outdated` and `update` command has several different version numbers. This is what they mean:

* **Package** - This contains the slug and semver range you've put in your `box.json` file.  i.e., what version you "asked" for.
* **Installed** - This is the exact version installed right now in your project
* **Update** - This is the newest possible version of the package _that satisfies the semver range in your `box.json`_.  Not there may be newer versions of the library not shown here depending on what your semver range is allowing.  A red highlight in this columns means a new version is available.
* **Latest** - This is the absolute latest stable version of this package regardless of your semver range.  In order to update to this version, you may need to run the `install` command again and ask specifically for it.  An orange highlight in this column means a new major update is available.

{% hint style="info" %}
Note, updating to a new major version of a library may contain breaking changes. This is why the default semver range is the caret (^) range which will prompt you with patch and minor updates, but NOT major updates. Those upgrades require manual intervention by default.
{% endhint %}

## ForgeBox Semantic Versioning Support

If you are using a ForgeBox package, the `update` command will comply with the semantic versioning range you specify. For example, if you have a dependency installed with a version saved of `^2.0.0` it will update you all the way to `2.9.9` but it will never install `3.0.0` until you ask it to. This is because breaking changes come in major versions, but minor releases are _supposed_ to be compatible.

## Determining Freshness

The way CommandBox determines whether there is a new version of a package differs based on the endpoint that installed the package. Versions are always treated as a semantic version (Major.Minor.Patch).

* **ForgeBox** - The ForgeBox REST API is used to get the latest package version.
* **HTTP(S)** - Package is always considered outdated, and re-downloaded.
* **File**  - The box.json's version is used from the zip. If box.json doesn't exist, the package is always considered outdated.
* **Folder** - The box.json's version is used from the folder. If box.json doesn't exist, the package is always considered outdated.
* **Git** - Package is always considered outdated, and re-cloned.

## External JSON Integration

If you want to integrate your package updates with an external process, you can get this data back as JSON so it can be parsed and used by another system.

```bash
CommandBox> outdated --JSON
Resolving Dependencies, please wait...
[
    {
        "SHORTDESCRIPTION":"This module provides server side validation to ColdBox applications",
        "VERSION":"1.0.0",
        "SLUG":"cbvalidation",
        "NAME":"ColdBox Validation",
        "DEV":false,
        "NEWVERSION":"1.0.3",
        "PACKAGEVERSION":"1.0.0"
    }
]
```
