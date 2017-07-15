# Using File Globs

If you want users to be able to pass file globbing patterns to your command, set a type of `Globber` for the argument.

```
/**
* @path.hint file or directory to interact with.
**/
function run( required Globber path )  { }
```

Even though the user types a string, CommandBox will hand you a CFC instance that's preloaded with the matching paths and has some nice methods to help you interact with the matching paths.  The Globber object lazy loads the matches, which gives you time to affect the pattern or even the sort if you wish prior to the file system actually being hit.

## Globber.count()

Use the `count()` method to get the total number of paths matched on the file system.
```
/**
* @path.hint file or directory to interact with.
**/
function run( required Globber path ) {
  print.line( path.count() & ' files affected!' );
}
```

## 