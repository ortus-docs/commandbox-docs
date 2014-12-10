# Developing Commands

CommandBox is extensible via CFML by creating command CFCs. Any CFC in the `user/.CommandBox/commands/` directory will be registered as a command as long as it extends `commandbox.system.BaseCommand` and has a `run()` method.

```javascript
component extends="commandbox.system.BaseCommand"{

    function run(){}
    
}
```

To create a two-part command called `testbox run` create CFCs that are nested in subfolders, for example: `user/.CommandBox/commands/testbox/run.cfc` Everything after the command is considered parameters.

## WireBox DI
All CFC's are wired via WireBox, so dependency injection and AOP are available to them.

## Tab Completion & Help
Tab completion and help are powered by metadata on these CFCs. If you would like to use a friendlier name for your command, add the attribute `aliases` to the component which is a comma-delimited list of names the command will be also known as.

```javascript
/**
* Help metadata
*/
component extends="commandbox.system.BaseCommand" aliases="luis"{

    function run(){}
    
}
```

## Example

Here is the `dir` command briefly explained:

```javascript
/**
* Lists the files and folders in a given directory. Defaults to current working directory
*
* {code:bash}
* dir samples
* {code} 
* 
**/
component extends="commandbox.system.BaseCommand" aliases="ls,ll,directory" excludeFromHelp=false {
 
    /**
    * @directory.hint The directory to list the contents of
    * @recurse.hint recursively list
    **/
    function run( String directory="", Boolean recurse=false ) {
    // command code goes here
    }
}
```

## ForgeBox Commands
If you package your custom commands and upload them to [ForgeBox](http://www.coldbox.org/forgebox), make sure to tag them as `commandbox-commands` type.  This will tell CommandBox automatically to install them in the custom commands folder for you when installing from ForgeBox.


