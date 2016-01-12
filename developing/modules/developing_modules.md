# Developing Modules

The unit of re-use in CommandBox is the module.  A module is a folder containing some settings and code that follows a convention.  Modules allow you you to develop your own additions and customizations to the tool without needing to modify the core code.  If you have worked with the ColdBox MVC framework, CommandBox modules are very similar to ColdBox modules.  If not, don't worry, they're very simple to learn. 

## Build Your First Module

A module is a folder that minimally contains a `ModuleConfig.cfc` file inside it.  User modules are stored in the `~/.CommandBox/cfml/modules/` directory on your hard drive.  Let's navigate to that folder and create a  new sub directory called `test`. Now inside of our new folder, create a new file called `ModuleConfig.cfc` and place the following in it.

**~/.CommandBox/cfml/modules/test/ModuleConfig.cfc**
```javascript
component {
    function configure(){}
    
    function onCLIStart() {
      shell.callCommand( 'upgrade' );
    }
}
```