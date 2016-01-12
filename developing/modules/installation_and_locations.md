# Installation and Locations

A module is nothing more than a folder that contains a `ModuleConfig.cfc` file.  The only requirement for that CFC is that it contains a method called `configure()`.  Modules can also contain models, interceptors, commands, and pretty much anything else you want to stick in them.  As long as they contain a `box.json` in their root, they are also a package, which means they are self-describing, can install dependencies, and can be installed via the `install` command.

## Locations

CommandBox has two default `modules` directories.  

* `~/.CommandBox/cfml/system/modules`
* `~/.CommandBox/cfml/modules`

The first is for system modules so you shouldn't need to touch it.  All the built-in commands are in those modules, so feel free to check out how they work.  All user-installed modules are in the second folder.  The `cfml` folder is the "root" of the CommandBox installation, so there will also be a `box.json` created in that folder once you start installing modules via the `install` command.


## Installation

The first way to create a module is to manually create a folder in the `~/.CommandBox/cfml/modules` directory and place your `ModuleConfig.cfc` inside of it.  This is the process described in the guide on [creating your first module](developing/modules/developing_modules.md).
