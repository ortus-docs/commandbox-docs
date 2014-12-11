# Developing Commands

CommandBox is extensible via CFML by creating command CFCs. Any CFC in the `user/.CommandBox/commands/` directory will be registered as a command as long as it extends `commandbox.system.BaseCommand` and has a `run()` method.

```javascript
component extends="commandbox.system.BaseCommand" {

    function run(){
        return 'Hello World!'; 
    }
    
}
```

To create a two-part command called `testbox run` create CFCs that are nested in subfolders, for example: `user/.CommandBox/commands/testbox/run.cfc` Everything after `testbox run` will be considered parameters.

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


