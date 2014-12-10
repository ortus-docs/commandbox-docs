# Tab Completion & Help

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