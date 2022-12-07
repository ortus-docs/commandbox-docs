# User Settings

The defaults for a module is stored in a `settings` struct in the `ModuleConfig.cfc` for that module. However, users can override those settings without needing to touch the core module code. This is made possible by special namespace in the CommandBox config settings called `modules`.

Consider the following module settings:

`/modules/TestModule/ModuleConfig.cfc`

```javascript
component{
    function configure(){
        settings = {
            mySetting = 'isCool',
            somethingEnabled = true
        };
    }
}
```

The following `config set` commands will create config settings that override those. The pattern is `modules.moduleName.settingName`.

```bash
config set modules.TestModule.mySetting=overridden
config set modules.TestModule.somethingEnabled=false
```

When a module is loaded, the config settings (that exist) for that module are also loaded and overwrite the defaults from `ModuleConfig.cfc`. You can easily see what settings are set for our `TestModule` like so:

```bash
config show modules.TestModule
```
