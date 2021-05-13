# What's New in 3.0.0

### Config Settings

CommandBox now has a JSON file of settings that can be used to configure any kind of behavior we want.  We're still working on implementing features that actually use the settings, but the first will be the ability to set up a proxy server for your corporate network. Config settings give us a place to store ad-hoc settings like this as well as an API for retrieving them.  What's great is the settings JSON file can store ANY information, including complex structs and arrays so feel free to use it for your own purposes as well.  You can interact with config settings like so:

```bash
config set name=mySetting
config set modules.myModule.mySetting=foo
config set myArraySetting[1]="value"
config set setting1=value1 setting2=value2 setting3=value3
config show settingName
```

[Read More](http://commandbox.ortusbooks.com/content/v/development/config_settings/config_settings.html)

### Modules

This is perhaps the most radical thing we've done in CommandBox to date and it is huge.  We've introduced modules \(just like ColdBox\) into the actual CLI itself.  A module is a unit of code re-use that allows you to take a folder of code that follows a few simple conventions and drop it into a module-aware application for instant extension.  This means that we've broken out all the internal commands into system modules for organization.  What's more, you can write your own CommandBox modules that hook into the internal workings with interceptors, register their own custom commands, or help manage settings or servers.  Modules can be placed on [ForgeBox](http://www.forgebox.io/) and installed by your friends in seconds to extend the core of CommandBox.  The benefits here can't be understated.  Check out [the docs](http://commandbox.ortusbooks.com/content/v/development/developing/modules/developing_modules.html) and go through the quick, easy steps to create your first CommandBox module.  

Not only can modules have settings, but you can also override a module's default settings easily with config settings that follow a simple convention.  For example, if a module named "**foo**" has a setting named "**bar**", you don't need to edit the module's code to change the setting.  Simply run this command:

```bash
config set modules.foo.bar=value
```

[Read More](http://commandbox.ortusbooks.com/content/v/development/developing/modules/developing_modules.html)

### Interceptors

CommandBox interceptors, like modules, work the same way that ColdBox interceptors do.  They give you hooks that you can register to listen to events broadcast by the CommandBox core, or custom events of your own design that you announce.  These are very powerful for being able to extend and modify how the core CLI works to build upon it.  Interceptors are bundled inside modules so they install quickly and easily.  Distribute them on [ForgeBox](http://www.forgebox.io/) as well.  I've already created a [simple example module](http://commandbox.ortusbooks.com/content/v/development/developing/example_project.html) that uses the **onCLIStart** interceptor to modify the ASCII art banner that appears when you start CommandBox.

Here's some of the core interception points:

* onCLIStart
* onCLIExit
* preCommand
* postCommand
* onServerStart
* onServerStop
* onException
* preInstall
* postInstall

Now you can write modules that check for upgrades on CommandBox startup, manipulate the output of commands, log exceptions, customize server startup, or audit what package you install the most!

[Read More](http://commandbox.ortusbooks.com/content/v/development/developing/interceptors/developing_interceptors.html)

### Standardized Command Packaging

We've had the ability for custom commands for a while, but they were limited and didn't easily allow you to include additional CFC files with your commands.  Now with the addition of modules, your custom commands can be package in a module right alongside settings, interceptors, or services.  We also simplified the creation of custom commands so things like extending our BaseCommand class is optional thanks to WireBox's virtual inheritance.  We hate boilerplate as much as you do!

There are already some cool custom commands popping up on ForgeBox.  Check out [this community addition](http://www.coldbox.org/forgebox/view/commandbox-http-command) for making http calls from the command line similar to curl.

```bash
install commandbox-http-command
```

[Read More](http://commandbox.ortusbooks.com/content/v/development/developing/commands/developing_commands.html)

### Server.json

This feature has been a long-time coming.  There are a lot of options you can set when starting a server, and portability has been hard for people wanting to distribute an app that needs to start with custom JVM args, rewrites, or a specific port. Now all server startup options can be set in your web root in a **server.json** file which will be used automatically the next time you run "start".  You interact with these settings the same as package or config settings.  

```bash
server set web.http.port=8080
server show web.http.port
server start
```

[Read More](http://commandbox.ortusbooks.com/content/v/development/embedded_server/serverjson.html)

### Shortcut for Native OS Binaries

We've had the "run" command for a while now that allows you to run native binaries from the interactive shell, or from a CommandBox recipe.  The output is returned which allows you to create mashups that pipe the output of OS commands directly into CommandBox commands.  We took this a step further and borrowed from other CLIs out there so now the parser allows you to call native OS binaries by simply prefixing an exclamation mark \(!\) in front of the binary name.  Now only are OS commands run in the current working directory, they are also executed via the shell for that machine which makes non-binaries and aliases like "ll" function.

```bash
!myApp.exe
!git pull
!dir
```

[Read More](http://commandbox.ortusbooks.com/content/v/development/usage/execution/os_binaries.html)

### Shortcut for CFML Functions via REPL

This is a really neat feature that allows you to actually run CFML functions straight from the CommandBox CLI as commands.  Just prefix the function name with a hash sign \(\#\) and then type the function name with no parenthesis.  Any parameters to the function can be passed \(or piped\) into the command like normal named or positional CLI command parameters.

```bash
#now
> {ts '2016-01-19 16:14:23'}
#hash mypass
> A029D0DF84EB5549C641E04A9EF389E5
#reverse abc
> cba
```

 This really gets cool when you start piping the output of commands together to string together mixtures of CommandBox commands and CFML functions for fancy one-liners.  Here's some string manipulation. The first one does some list manipulation.  The second one outputs the lowercase package name.

```bash
#listGetAt www.foo.com 2 . | #ucase | #reverse
> OOF
package show name | #lcase
> my package
```

But wait, there's more! You can even use struct and array functions.  Their output is returned as JSON and automatically deserialized as input to the next command.  Keep in mind that piped data gets passed in as the FIRST parameter to the next command.  This outputs a nice list of all the top-level dependencies in your package.

```bash
package list --JSON | #structFind dependencies | #structKeyList
> coldbox,docbox,testbox,cbmessagebox
```

[Read More](http://commandbox.ortusbooks.com/content/v/development/usage/execution/cfml_functions.html)

### Expressions in Command Parameters

Parameter values passed into a CommandBox command don't have to be static. Any part of the parameter which is enclosed in backticks \(\`\) will be evaluated as a CommandBox expression. That means that the enclosed text will be first executed as though it were a separate command and the output will be substituted in its place.  

```bash
echo "Your CommandBox version is `ver` and this app is called '`package show name`'!!"
> Your CommandBox version is 3.0.0 and this app is called 'Brad's cool app'!
```

You can really go crazy with these mashups by combining CFML functions too.  This example sets a property in a package's box.json that's equal to a nicely formatted date:

```bash
package set createdDate=`#now | #dateformat mm/dd/yyyy`
> Set createdDate = 1/19/2016
```

[Read More](http://commandbox.ortusbooks.com/content/v/development/usage/parameters/expressions.html)

### Command DSL

We've really focused on doing CommandBox development now with the possibilities opened up with the addition of modules.  One pain point of extending CommandBox was calling other commands since parameters needed to be escaped.  We created a nice method-chaining DSL to help execute any other command from inside of your custom commands.

```javascript
command( 'cp' )
    .params( path='/my/path', newPath='/my/new/path' )
    .run();
```

You can even nest the DSL to pipe output between commands:

```javascript
command( "echo" )
    .params( "hello#chr( 10 )#world" )
    .pipe( 
        command( "grep" )
        .params( "lo" )
    )
    .run();
```

[Read More](http://commandbox.ortusbooks.com/content/v/development/developing/commands/running_other_commands.html)

### New WireBox Injection DSLs

In line with the previous item, we've made it yet easier to write custom modules that extend the functionality of CommandBox by adding new WireBox injection DSLs.  Everything inside of CommandBox is created and autowired by WireBox.  You can now ask WireBox to inject core services, module settings, or config settings.

```javascript
property name='shell'  inject='commandbox';
property name='ModuleService'   inject='ModuleService';
property name='MyService' inject='MyService@MyModule';
property name='mySetting' inject='commandbox:moduleSettings:moduleName:mySetting';
property name='myConfigSetting' inject='commandbox:ConfigSettings:myConfigSetting'
```

[Read More](http://commandbox.ortusbooks.com/content/v/development/developing/injection_dsl.html)

### Release Notes

#### **Bug**

* \[[COMMANDBOX-144](https://ortussolutions.atlassian.net/browse/COMMANDBOX-144)\] - Starting a server by short name doesn't work
* \[[COMMANDBOX-285](https://ortussolutions.atlassian.net/browse/COMMANDBOX-285)\] - Tag REPL seems to be unavailable
* \[[COMMANDBOX-300](https://ortussolutions.atlassian.net/browse/COMMANDBOX-300)\] - REPL output cannot be piped
* \[[COMMANDBOX-305](https://ortussolutions.atlassian.net/browse/COMMANDBOX-305)\] - url rewriterules in commandbox incorrect
* \[[COMMANDBOX-310](https://ortussolutions.atlassian.net/browse/COMMANDBOX-310)\] - Brew fomulas SHA1 mismatch
* \[[COMMANDBOX-317](https://ortussolutions.atlassian.net/browse/COMMANDBOX-317)\] - Starting server from OS shell doesn't always work
* \[[COMMANDBOX-321](https://ortussolutions.atlassian.net/browse/COMMANDBOX-321)\] - Restarting server saves openBrowser as false in server.json
* \[[COMMANDBOX-324](https://ortussolutions.atlassian.net/browse/COMMANDBOX-324)\] - Can't clear JSON properties with dash in the name
* \[[COMMANDBOX-326](https://ortussolutions.atlassian.net/browse/COMMANDBOX-326)\] - Coldbox create view commands break if name includes package
* \[[COMMANDBOX-333](https://ortussolutions.atlassian.net/browse/COMMANDBOX-333)\] - CommandBox TestBox array of runners does not run

#### **New Feature**

* \[[COMMANDBOX-52](https://ortussolutions.atlassian.net/browse/COMMANDBOX-52)\] - Provide a more programmatic way to run commands/tasks like a method
* \[[COMMANDBOX-111](https://ortussolutions.atlassian.net/browse/COMMANDBOX-111)\] - Allow native binary execution with exclamation mark
* \[[COMMANDBOX-266](https://ortussolutions.atlassian.net/browse/COMMANDBOX-266)\] - Add server.json to default server settings
* \[[COMMANDBOX-291](https://ortussolutions.atlassian.net/browse/COMMANDBOX-291)\] - Global CommandBox setting file
* \[[COMMANDBOX-294](https://ortussolutions.atlassian.net/browse/COMMANDBOX-294)\] - Disable sendfile in runwar server
* \[[COMMANDBOX-296](https://ortussolutions.atlassian.net/browse/COMMANDBOX-296)\] - Refactor JSON handling out of package commands for reuse
* \[[COMMANDBOX-302](https://ortussolutions.atlassian.net/browse/COMMANDBOX-302)\] - Allow \# as a REPL shortcut to run CFML tags or functions
* \[[COMMANDBOX-303](https://ortussolutions.atlassian.net/browse/COMMANDBOX-303)\] - Allow expressions in command parameters
* \[[COMMANDBOX-306](https://ortussolutions.atlassian.net/browse/COMMANDBOX-306)\] - Add option to server start for enabling/disabling directory browsing module
* \[[COMMANDBOX-312](https://ortussolutions.atlassian.net/browse/COMMANDBOX-312)\] - Allow commands to be piped in to box
* \[[COMMANDBOX-313](https://ortussolutions.atlassian.net/browse/COMMANDBOX-313)\] - Override module settings with config settings on module load
* \[[COMMANDBOX-314](https://ortussolutions.atlassian.net/browse/COMMANDBOX-314)\] - WireBox injection DSLs for module config and settings
* \[[COMMANDBOX-319](https://ortussolutions.atlassian.net/browse/COMMANDBOX-319)\] - Handle struct of environment variables in server.json
* \[[COMMANDBOX-320](https://ortussolutions.atlassian.net/browse/COMMANDBOX-320)\] - Add additional helper reference to the Executor

#### **Task**

* \[[COMMANDBOX-301](https://ortussolutions.atlassian.net/browse/COMMANDBOX-301)\] - Remove bleeding edge builds from production Debian repo.

#### **Improvement**

* \[[COMMANDBOX-121](https://ortussolutions.atlassian.net/browse/COMMANDBOX-121)\] - Standardize command packaging
* \[[COMMANDBOX-205](https://ortussolutions.atlassian.net/browse/COMMANDBOX-205)\] - HTTP Calls don't work behind company proxy
* \[[COMMANDBOX-226](https://ortussolutions.atlassian.net/browse/COMMANDBOX-226)\] - Use virtual inheritance for commands
* \[[COMMANDBOX-250](https://ortussolutions.atlassian.net/browse/COMMANDBOX-250)\] - Allow ad-hoc JVM args when starting server
* \[[COMMANDBOX-251](https://ortussolutions.atlassian.net/browse/COMMANDBOX-251)\] - Add modularity
* \[[COMMANDBOX-252](https://ortussolutions.atlassian.net/browse/COMMANDBOX-252)\] - Add event-listener model to CommandBox
* \[[COMMANDBOX-284](https://ortussolutions.atlassian.net/browse/COMMANDBOX-284)\] - Improve error handling in CFLib endpoint
* \[[COMMANDBOX-286](https://ortussolutions.atlassian.net/browse/COMMANDBOX-286)\] - Alias Execute as exec
* \[[COMMANDBOX-287](https://ortussolutions.atlassian.net/browse/COMMANDBOX-287)\] - Return better details from progressable downloader
* \[[COMMANDBOX-288](https://ortussolutions.atlassian.net/browse/COMMANDBOX-288)\] - Run command uses same environment as box executable when it was first started
* \[[COMMANDBOX-289](https://ortussolutions.atlassian.net/browse/COMMANDBOX-289)\] - Improve error handling in HTTP endpoint
* \[[COMMANDBOX-290](https://ortussolutions.atlassian.net/browse/COMMANDBOX-290)\] - Update to latest version of WireBox
* \[[COMMANDBOX-292](https://ortussolutions.atlassian.net/browse/COMMANDBOX-292)\] - bump command reset minor and patch
* \[[COMMANDBOX-297](https://ortussolutions.atlassian.net/browse/COMMANDBOX-297)\] - bump runwar version to 3.3.0
* \[[COMMANDBOX-298](https://ortussolutions.atlassian.net/browse/COMMANDBOX-298)\] - Enhance server rewrites for file/dir detection
* \[[COMMANDBOX-307](https://ortussolutions.atlassian.net/browse/COMMANDBOX-307)\] - Refactor core commands to be modules
* \[[COMMANDBOX-308](https://ortussolutions.atlassian.net/browse/COMMANDBOX-308)\] - Move application templates into the coldbox-commands module
* \[[COMMANDBOX-309](https://ortussolutions.atlassian.net/browse/COMMANDBOX-309)\] - Move scaffolding templates into respective modules
* \[[COMMANDBOX-311](https://ortussolutions.atlassian.net/browse/COMMANDBOX-311)\] - Improve error handling in commands
* \[[COMMANDBOX-315](https://ortussolutions.atlassian.net/browse/COMMANDBOX-315)\] - Shortcut to cd into directory after mkdir command
* \[[COMMANDBOX-322](https://ortussolutions.atlassian.net/browse/COMMANDBOX-322)\] - Run command doesn't run in the same CWD as CommandBox
* \[[COMMANDBOX-323](https://ortussolutions.atlassian.net/browse/COMMANDBOX-323)\] - Allow run command to run any OS command from the shell
* \[[COMMANDBOX-325](https://ortussolutions.atlassian.net/browse/COMMANDBOX-325)\] - Allow ConfigService to use nested setting keys
* \[[COMMANDBOX-327](https://ortussolutions.atlassian.net/browse/COMMANDBOX-327)\] - Improve parsing of run and ! command
* \[[COMMANDBOX-328](https://ortussolutions.atlassian.net/browse/COMMANDBOX-328)\] - Allow user to set custom shell with config setting
* \[[COMMANDBOX-330](https://ortussolutions.atlassian.net/browse/COMMANDBOX-330)\] - warn user if package has invalid JSON file
* \[[COMMANDBOX-331](https://ortussolutions.atlassian.net/browse/COMMANDBOX-331)\] - Bump JRE version to 1.8.0\_72
* \[[COMMANDBOX-332](https://ortussolutions.atlassian.net/browse/COMMANDBOX-332)\] - Embedded server doesn't sent proper headers for SVGZ files
* \[[COMMANDBOX-334](https://ortussolutions.atlassian.net/browse/COMMANDBOX-334)\] - Support ContentBox installation paths

