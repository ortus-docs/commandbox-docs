# Setting Sync

If you are authenticated to ForgeBox in the CLI, you can syncronize your config settings to and from your ForgeBox account.  This is a great way to get up and running on a new PC, or keep multiple CommandBox installs in sync with each other.  In addition to synchronizing your Config Settings, this feature will also track your installed system modules, such as CFConfig, etc.

In order to sync your settings and modules, you must first be logged into ForgeBox.  Check and see if you are logged in with

```bash
forgebox whoami
```

If necessary, log in with

```
forgebox login
```

Now you can synchronize your settings.

### config sync push

This command will push your local settings and modules up to your ForgeBox account.

```bash
config sync push
```

By default, the settings are "merged" so new local settings will be added to ForgeBox, but nothing will be removed.  In order to remove config that only exists on ForgeBox, you can use the `--overwrite` flag to force a full sync.

```bash
config sync push --overwrite
```

### config sync pull

This command will pull your settings and modules from your ForgeBox account and set/install them locally.

```bash
config sync pull
```

By default, the settings are "merged" so missing settings will be added locally and missing system modules will be installed, but nothing will be removed.  In order to remove config and modules that only exists locally, you can use the `--overwrite` flag to force a full sync.  This will remove local config settings and uninstall local system modules which were not on ForgeBox.

```bash
config sync pull --overwrite
```

### config sync diff

This command will not change anything, but gives you a full report of all settings which are different between your local CommandBox CLI and ForgeBox.  It will show you "Remote Only", "Local Only", and "Changed" settings and modules.  Use this to see what you're about to change prior to pushing or pulling.

```bash
config sync diff
```

