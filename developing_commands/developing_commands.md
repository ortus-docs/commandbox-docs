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


## WireBox DI
All CFC's are wired via WireBox, so dependency injection and AOP are available to them.  This can be handy for commands to wrap services provided by models, or to access utilities and services inside CommandBox.





