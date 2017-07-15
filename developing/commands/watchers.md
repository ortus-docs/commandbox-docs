# Watchers

CommandBox has a powerful utility that can be used to watch a folder of files for changes and fire arbitrary code when changes happen. The utility will block execution of the command until the user stops it with `Ctrl+C`.  To use a watcher in your command, there is a method called `watch()` in the base command class that you can call.  It has a nice DSL of chainable methods to configure it.

```
watch()
	.paths( '**.cfc' )
	.inDirectory( getCWD() )
	.withDelay( 5000 )
	.onChange( function() {
	
		print.line( 'Something changed!' );
		command( 'testbox run' )
			.run();
			
	} )
	.start();
```

Here's a rundown of the methods used above in the DSL.

* **paths( ... )** - Receives a comma-delimtied list of globbing patterns to watch for changes. (defaults to `**`)
* **inDirectory( ... )** - Set the base directory that the file globs are relative to. (defaults to current working directory)
* **withDelay( ... )** - Set the number of milliseconds between polling the file system. (defaults to 500 ms)
* **onChange( ... )** - Pass a closure to be executed when a change has occurred.
* **start()** - Starts the watcher. Always call this at the end of the DSL chain