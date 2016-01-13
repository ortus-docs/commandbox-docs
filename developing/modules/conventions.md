# Module Conventions

What makes modules so easy to use is the convention-over-configuration approach that lets you use expected file and folder names instead of manually configuring things.  Here are some of the high-level conventions that modules use.

# Configuration

The settings for a module is stored in the `ModuleConfig.cfc` file in the root of the module.  This CFC is automatically created and its settings extracted when the module is loaded.  It has lifecycle methods like `onLoad()` and `onUnLoad()` that will be called automatically if they exist.  This CFC will also be loaded up as an interceptor, so any method names that match interception points will be registered.   [Read more about module configuration here](/developing/modules/configuration.md).

# Models

# Commands

# Interceptors

# Modules