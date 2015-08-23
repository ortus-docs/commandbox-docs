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
