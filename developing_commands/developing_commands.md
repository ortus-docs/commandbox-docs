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





