# Wirebox Injection DSL

Everything inside CommandBox is built via Wirebox. This means your models, commands, and interceptors can all take advantage of Wirebox's dependency injection.   All you need to do is add a property to the top of your CFC that tells WireBox what to inject.

## Core Models

There are a number of automatically-mapped objects inside WireBox.  Here are some examples:

```javascript
property name='shell'           inject='commandbox';
property name='tempDir'         inject='tempDir@constants';
property name='formatterUtil'   inject='formatter';
property name='ModuleService'   inject='ModuleService';
property name='ConfigService'   inject='ConfigService';

```

## Custom Models

Any CFCs in the `models` folder of any module will automatically be mapped in WireBox with the pattern `modelname@modulename`.  Therefore, if you create a module called "SuperDuper" that has a CFC called `models/AmazingService.cfc`, you could inject it in your module's custom command like so:

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
```

## CommandBox Config

You can also inject Commandbox config settings into your CFCs.  Only inject a specific setting if you expect it to exist.  If you need to check for its existence (and provide a default), then inject the entire settings struct.