# Installation and Locations

A module is nothing more than a folder that contains a `ModuleConfig.cfc` file.  The only requirement for that CFC is that it contains a method called `configure()`.  Modules can also contain models, interceptors, commands, and pretty much anything else you want to stick in them.  As long as they contain a `box.json` in their root, they are also a package, which means they are self-describing, can install dependencies, and can be installed via the `install` command.

## Locations

CommandBox has two default `modules` directories.  

* `~/.CommandBox/cfml/system/modules`
* `~/.CommandBox/cfml/modules`



## Installation

