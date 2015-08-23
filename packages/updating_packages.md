# Updating packages

CommandBox does more than help yo install packages.  It also helps you keep them up to date as well.  Remember, you can always get a quick list of all the dependencies installed your app with the `list` command.

```bash
CommandBox> list
Dependency Hierarchy myApp (1.0.0)
├── coldbox (4.2.0)
└─┬ cbvalidation (1.0.0)
  └── cbi18n (1.0.0)
```

## Getting the Latest

To check and see if any of your installed packages can be updated to a newer version, run the `update` command.

```bash
CommandBox> update
Resolving Dependencies, please wait...
Found (1) Outdated Dependency
* cbvalidation (1.0.0) ─> new version: 1.0.3
Would you like to update the dependency? (yes/no) :
```

Entering "yes" will install the newest version of the package.  It is also possible to get a list of outdated dependencies without the prompt to update them with the `outdated` command.

```bash
outdated
```

## Determining Freshness

The way CommandBox determines whether there is a new version of a package differs based on the endpoint that installed the package.  Versions are always treated as a semantic version (Major.Minor.Patch).

* **ForgeBox** - The ForgeBox REST API is used to get the latest package version
* **HTTP(S)** - Package is always considered outdated, and re-downloaded.
* **File**  - The box.json's version is used from the zip. If box.json doesn't exist, the package is always considered outdated.
* **Folder** - The box.json's version is used from the folder. If box.json doesn't exist, the package is always considered outdated.
* **Git** - Package is always considered outdated, and re-cloned.

## External JSON Integration


If you want to integrate your package updates with an external process, you can get this data back as JSON so it can be parsed and used by another sytem.


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

