# Injection DSL

Everything inside CommandBox is built via Wirebox. This means your models, commands, and interceptors can all take advantage of Wirebox's dependency injection. All you need to do is add a property to the top of your CFC that tells WireBox what to inject.

## Core Models

There are a number of automatically-mapped objects inside WireBox. Here are some examples:

```javascript
property name='shell'           inject='commandbox';
property name='tempDir'         inject='tempDir@constants';
property name='formatterUtil'   inject='formatter';
property name='ModuleService'   inject='commandbox:ModuleService';
property name='ConfigService'   inject='ConfigService';
property name='InterceptorService'   inject='commandbox:interceptorService';
property name='AsyncManager'   inject='commandbox:asyncManager';
```

## Custom Models

Any CFCs in the `models` folder of any module will automatically be mapped in WireBox with the pattern `modelname@modulename`. Therefore, if you create a module called "SuperDuper" that has a CFC called `models/AmazingService.cfc`, you could inject it in your module's custom command like so:

```javascript
property name='AmazingService' inject='AmazingService@SuperDuper';
```

## Module Config and Settings

If a module's service or command wants to access configuration data or module settings, they can be injected using these injection DSLs.

```javascript
property name='allModuleConfigs'    inject='commandbox:moduleConfig';
property name='moduleConfig'        inject='commandbox:moduleConfig:moduleName';
property name='moduleSettings'      inject='commandbox:moduleSettings:moduleName';
property name='mySetting'           inject='commandbox:moduleSettings:moduleName:mySetting';
property name='myDeepSetting'       inject='commandbox:moduleSettings:moduleName:mySetting';
```

## CommandBox Config

You can also inject Commandbox config settings into your CFCs. Only inject a specific config setting if you expect it to exist. If you need to check for its existence (and provide a default), then inject the entire settings struct. You can also inject nested keys of a complex config setting directly like the last line shows.

```javascript
property name='allConfigSettings'   inject='commandbox:ConfigSettings';
property name='myConfigSetting'     inject='commandbox:ConfigSettings:myConfigSetting';
property name='myDeepConfigSetting' inject='commandbox:ConfigSettings:myConfigSetting.nested.keys.here';
```

## box Injection Namespace

In order to provide compatibility for generic modules to work the same between Coldbox MVC and CommandBox CLI, we have introduced a new injection namespace called `box` which is simply an alias for `commandbox`. All of the examples above such as

```javascript
property name='moduleSettings' inject='commandbox:moduleSettings:moduleName';
```

can also be written as

```javascript
property name='moduleSettings' inject='box:moduleSettings:moduleName';
```

and will allow your module to work the same when installed to ColdBox MVC or CommandBox CLI. note the `box` injection namespace was added to CommandBox in version `4.7.0` and added to ColdBox MVC in version `5.4.0` so using it will preclude the use of your module on older versions of either platform.

## Reloading Changes

CFProperty injections are aggressively caches on disk to survive restarts. If you change the injections on a command CFC, you will need to run the `reload` command (aliased as `r`) to reprocess metadata on your CFCs.
