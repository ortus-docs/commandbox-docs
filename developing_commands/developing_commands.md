# Developing Commands

CommandBox is extensible via CFML by creating modules that contain command CFCs. This is a very easy and powerful way to create custom, reusable CFML scripts that interact with the user, automate hard tasks, and can be shared with other developers.  Let's create a "Hello World" command.


## Your First Command

To create our first command, we'll need a new module.  A module can contain as many commands as you like.  You can create a module by placing a folder in `~/.CommandBox/cfml/modules/` that contains a `ModuleConfig.cfc` file.  The minimum contents of your module config is:

**modules/test/ModuleConfig.cfc**
```javascript
component {
    function configure(){}
}
```
Now, create a `commands` folder inside your module for your command to live in.  Each CFC in this folder will be registered as a command.  The only requirement for a command CFC is that is has a `run()` method.

**modules/test/commands/Hello.cfc**
```javascript
component {
    function run(){
        return 'Hello World!'; 
    }
}
```

That's it!  After creating your module, run the `reload` command from the shell, and then the name of the new command is the same as the name of the CFC.  In this case, you would run the command above like so:

```bash
hello
```

It would output `Hello World!` to the console.  Anything after `hello` will be passed to your `run()` function as parameters.  

## Create Namespaces

To create a two-part command like `say hello` create CFCs that are nested in subfolders, for example: `~/.CommandBox/cfml/modules/test/commands/say/Hello.cfc` The contents of the `Hello.cfc` would not change, but the namespace will match the folder name by convention.  The namespaced command would be called like so:  
```bash
say hello
```

There is not a limit to how deeply you can nest your namespace folders.  CommandBox's built in help and tab-completion will always work via conventions.

## Making Changes

Commands are created and stored once for the duration that CommandBox is running.  If testing changes to a command in the interactive shell, use the `reload` command (aliased as `r`) to reload the shell.  Your changes will immediately be available.  Using the up arrow to access the shell's history can also be useful here.


## WireBox DI

All CFCs including commands are created and wired via WireBox, so dependency injection and AOP are available to them.  This can be handy for commands to wrap services provided by models, or to access utilities and services inside CommandBox.

This command would inject CommandBox's ArtifactService to list out all the packages being stored.

```javascript
component extends="commandbox.system.BaseCommand" {

	property name='artifactService' inject='artifactService'; 
	
    function run(){
        var results = artifactService.listArtifacts();
		for( var package in results ) {
			print.boldCyanLine( package );
		}
    }
    
}
```

Commands also have a `variables.wirebox` variable as well as their own `getInstance()` method which proxies to WireBox to get objects.

```javascript
var results = getInstance( 'artifactService' ).listArtifacts();
```


