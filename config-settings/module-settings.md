# Module Settings

These settings affect how CommandBox loads modules.

## ModulesExternalLocation

**array**

You can store CommandBox modules outside of the default installation directory. This may be useful to point to modules you are developing or to keep custom modules around even if CommandBox gets uninstalled.

```bash
config set ModulesExternalLocation=[\"/var/my/external/modules\"]
config show ModulesExternalLocation
```

## modulesInclude

**array**

An array of module names to load. Be careful of using this setting as once you set it, no other modules will be loaded which includes all of CommandBox's core modules.

```bash
config set modulesInclude=[\"moduleName\",\"anotherModuleName\"]
config show modulesInclude
```

## ModulesExclude

**array**

An array of module names NOT to load. This can be useful when you have an installed module that's erroring on load and preventing CommandBox from starting up.

```bash
config set ModulesExclude=[\"moduleName\",\"anotherModuleName\"]
config show ModulesExclude
```

## modules.\*

**struct**

When you install a CommandBox module, it may contain settings that affect how it works. Don't edit the CFML code in the module, instead use the `config set` command to create config settings that will override the module's defaults. The pattern is `modules.moduleName.settingName`.

```bash
config set modules.TestModule.mySetting=overridden
config set modules.TestModule.somethingEnabled=false
```

When a module is loaded, the config settings (that exist) for that module are loaded as well. Any time you set a new module setting, that setting will be loaded into memory immediately for that module.

You can easily see what settings are set for our `TestModule` like so:

```bash
config show modules.TestModule
```
