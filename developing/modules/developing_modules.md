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

This module runs the `upgrade` command every time the CLI starts up to check and see if you have the latest version.  Pretty cool, huh?  

Let's take it a step further.  Since this upgrade check could get annoying, let's only run it if we're starting the shell in interactive mode (with the blinking cursor awaiting input).   That way, one-off commands from the OS shell that exit right away won't trigger the update check.  Our `onCLIStart()` method gets an argument called `intercept` data that has a flag for this.  

```javascript
function onCLIStart( interceptData ) {
  if( interceptData.shellType == 'interactive' ) {
    shell.callCommand( 'upgrade' );
  }
}
```

One final improvement.  Create a module setting called `checkForUpdates` which defaults to `true`.  Users can set it to `false` to disable the update check.  Here is the final version of the `ModuleConfig.cfc`:

```javascript
component {
    function configure(){
		settings = {
			checkForUpdates=true
		};
    }

    function onCLIStart( interceptData ) {
        if( interceptData.shellType == 'interactive' && settings.checkForUpdates ) {
            shell.callCommand( 'upgrade' );
        }
    }
}```

Now run the following command to override our module's default setting and turn off the update check.
```bash
config set modules.mytest.checkforupdates=false
```
Reload the shell and you'll see the update check is gone.  