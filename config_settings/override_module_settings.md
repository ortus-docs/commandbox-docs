# Override Module Settings

When you install a CommandBox module, it may contain settings that affect how it works.  Don't edit the CFML code in the module, instead use the `config set` command to create config settings that will override the module's defaults.  The pattern is `modules.moduleName.settingName`.

```bash
config set modules.TestModule.mySetting=overridden
config set modules.TestModule.somethingEnabled=false
```

When a module is loaded, the config settings (that exist) for that module are loaded as well.  Any time you set a new module setting, that setting will be loaded into memory immediately for that module.  

You can easily see what settings are set for our `TestModule` like so:

```bash
config show modules.TestModule
```