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