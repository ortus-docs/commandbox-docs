# Watchers

CommandBox has a powerful utility that can be used to watch a folder of files for changes and fire arbitrary code when changes happen. The utility will block execution of the task until the user stops it with `Ctrl+C`. To use a watcher in your task, there is a method called `watch()` in the base task class that you can call. It has a nice DSL of chainable methods to configure it.

```javascript
watch()
    .paths( '**.cfc' )
    .excludePaths( '/config/Coldbox.cfc,/config/WireBox.cfc' )
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

* **paths( ... )** - Receives one or more globbing patterns to watch for changes. Pass each globbing pattern as a separate argument. (defaults to `**`)
* **inDirectory( ... )** - Set the base directory that the file globs are relative to. (defaults to current working directory)
* **withDelay( ... )** - Set the number of milliseconds between polling the file system. (defaults to 500 ms)
* **onChange( ... )** - Pass a closure to be executed when a change has occurred.
* **start()** - Starts the watcher. Always call this at the end of the DSL chain

## onChange() Closure

If you don't care what the change was then you don't need to define any arguments to your closure. However, each time your closure is called, there is a struct of data passed to it that defines what paths were added removed and changed. The data is the format of:

```javascript
{
  'added':[],
  'removed':[],
  'changed':[] 
}
```

The arrays will contain the corresponding file paths. For example, if the `removed` array is empty, it means no files were removed. There should be at least one file path in at least one of the 3 arrays.

```javascript
watch()
    .onChange( function( paths ) {
        print
            .line( '#paths.added.len()# paths were added!' )
            .line( '#paths.removed.len()# paths were removed!' )
            .line( '#paths.changed.len()# paths were changed!' )            ;
    } )
    .start();
```
