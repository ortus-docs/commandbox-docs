# Advanced Installation

In the CFML world, there are no global conventions for where to install things to nor where to store dependencies.  Therefore, CommandBox for the most part will just stick packages in the root of your site unless you tell it otherwise.  It may not be pretty, but it's as good as stock CFML apps can get.  That means a lot of the cool things other package managers like NPM can do simply won't be available to you.

If you're using the ColdBox MVC Platform, congratulation!  You just unlocked **advanced mode**!  ColdBox uses *conventions* that tells you where to put stuff, and most importantly it has *modularity as a first class citizen*.  Not only that, but modules can be nested infinitely to nicely encapsulate dependencies and WireBox will automatically find and register each module's models for your application to use.

## Module Conventions

Modules are basically smart packages and when CommandBox installs or uninstalls modules it will behave a bit differently to take advantage of the functionality only available to the CFML world via the ColdBox Platform.

When installing a module, it will be placed in the modules/ directory.  That means the cbvalidation module will install here:

```
<webroot>/modules/cbvalidation
```

The cbvalidation module has dependencies of its own but it is an island unto itself and will encapsulate these.  Therefore the cbi18n module will be installed in a modules/ folder inside cbvalidation.

```
<webroot>/modules/cbvalidation/modules/cbi18n
```

You'll be able to see a nice representation of this when you use the `list` command.

```bash
CommandBox> list
Dependency Hierarchy myApp (1.0.0)
├── coldbox (4.2.0)
└─┬ cbvalidation (1.0.0)
  └── cbi18n (1.0.0)
```

# Moduleplicity

What this opens the door for is more than one module to depend on different versions of the same second module.  Both can be installed and nested under the respective parent.  In the near future, WireBox will be smart enough to present these nested modules only to their parents so they are fully encapsulated.  

The idea is that a module can "see" and use another module installed at the same level or higher in the hierarchy, but not lower.  That makes dependencies a bit of a black box to their parents.  This also allows us to bypass some redundancy.  For instance, when installing a module, if a satisfying version of that module already exists at a higher level, we skip the installation.  Consider this example:


```bash
install cbi18n
install cbvalidation
```

We know that cbvaliation requires cbi18n, but since it is already installed in the root modules folder, we won't install it again under cbvalidation.

# Smart Uninstall

The last way modules are better than sliced papusas is in how they uninstall.  We talked about how non-modules install-- just littered in the web root in a jumble of dependencies.  When uninstalling a non-module package, CommandBox will recursively go through the dependencies and remove them as well.  However, when uninstalling a module, that module's folder is simply deleted and that's it.  Since all dependencies are contained in the "black box", there's no need to go hunting for them. 




