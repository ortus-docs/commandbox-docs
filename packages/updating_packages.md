# Updating packages

CommandBox does more than help yo install packages.  It also helps you keep them up to date as well.  Remember, you can always get a quick list of all the dependencies installed your app with the `list` command.

```bash
CommandBox> list
Dependency Hierarchy myApp (1.0.0)
├── coldbox (4.2.0)
└─┬ cbvalidation (1.0.0)
  └── cbi18n (1.0.0)
```

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

If you want to integrate your package updates with an external process, you can get this data back as JSON so it can be parsed and used by another sytem.


```bash
outdated --JSON
```