# Module Conventions

What makes modules so easy to use is the convention-over-configuration approach that lets you use expected file and folder names instead of manually configuring things.  Here are some of the high-level conventions that modules use.

# Configuration

```
/modules/ModuleName/ModuleConfig.cfc
```

The settings for a module is stored in the `ModuleConfig.cfc` file in the root of the module.  This CFC is automatically created and its settings extracted when the module is loaded.  It has lifecycle methods like `onLoad()` and `onUnLoad()` that will be called automatically if they exist.  This CFC will also be loaded up as an interceptor, so any method names that match interception points will be registered.   [Read more about module configuration here](/developing/modules/configuration.md).

# Models

```
/modules/ModuleName/models/
```

CommandBox ships with WireBox, which is the glue that holds everything together.  Your custom modules can contain as many models as they like.  Place your CFCs (beans, services, etc) in the `models` folder and WireBox will automatically map them as `modelName@moduleName` so they can easily be injected into any other model, interceptor, or command CFC.

# Commands

```
/modules/ModuleName/commands/
```

One of the primary purposes of modules is to serve as a delivery mechanism for custom commands.  All command CFCs should be placed in the `commands` folder inside your module.  Any CFCs there will automatically be registered when the module is loaded, and will be available in the `help` command, as well as tab-completion, etc.  To create namespace commands, create extra sub folders inside the `commands` folder that match the name of the namespace.

# Interceptors

# Modules