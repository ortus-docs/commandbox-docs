# Watchers

CommandBox has a powerful utility that can be used to watch a folder of files for changes and fire arbitrary code when changes happen. The utility will block execution of the command until the user stops it with `Ctrl+C`. To use a watcher in your command, there is a method called `watch()` in the base command class that you can call. It has a nice DSL of chainable methods to configure it.

```text
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

* **paths\( ... \)** - Receives a comma-delimtied list of globbing patterns to watch for changes. \(defaults to `**`\)
    Note that the "paths" here work more like `.gitignore` entries and less like bash paths. Specifically:
    - A path with a leading slash (or backslash), will be evaluated relative to the current working directory. E.g. `watch /foo` will only watch files in the directory at `./foo`, but not in directories like `./bar/foo`.
    - A path without a leading slash (or backslash) will be applied as a glob filter to *all files* within the current working directory. E.g. `watch foo` will result in the entire working directory being watched, but only files matching the glob `**foo` will be processed.

    If your watcher seems slow, unresponsive, or is failing to notice some file change events, it is likely that you have it watching too many files. Try specifying more specific paths to the files you want to process, and use leading slashes in your arguments to avoid watching all files in the current working directory.
* **inDirectory\( ... \)** - Set the base directory that the file globs are relative to. \(defaults to current working directory\)
* **withDelay\( ... \)** - Set the number of milliseconds between polling the file system. \(defaults to 500 ms\)
* **onChange\( ... \)** - Pass a closure to be executed when a change has occurred.
* **start\(\)** - Starts the watcher. Always call this at the end of the DSL chain

