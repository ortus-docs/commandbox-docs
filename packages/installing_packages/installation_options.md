# Installation Options

There are several options you can use to control how a package is installed.

## Saving dependencies

By default, any package you install will be saved as a dependency.  To save it as a development dependency instead, use the `--saveDev` flag.

```bash
install testbox --saveDev
```

If you DON'T want the package you're installing to be saved as a dependency pass `save=false` or negate the save flag as  `--!save`.

```bash
install cborm --!save
```

## Production Installation

When you install a package, all dependencies will be installed.  If you want to skip development dependencies, use the `--production` flag.  This will also cause CommandBox to obey the package's ignoreList property in its box.json.  

```bash
install cbvalidation --production
```

## Verbose Installation

If you're a glutton for information, or perhaps you just want to debug what's going on, set the `--verbose` flag to get extra debugging information out of the install command including a list of every file that's installed.

```bash
install cbi18n --verbose
```

## Forced Installation

If CommandBox sees the directory that it was going to install into already exists with a newer or equal version of the package inside, it will decline to install again since it would be overwriting what's already there.  If you want to install anyway, use the `--force` flag.

```bash
install cbsoap --force
```




