# What's New in 3.3.0

### Server Enhancements

The embedded CommandBox server have seen a number of nice enhancements to make it easier for you to use CommandBox for super easy local development.

#### Fusion Reactor Module

The more people begin to use CommandBox for local development, the more interested they became in being able to run [FusionReactor](http://www.fusion-reactor.com/?affiliate=ortussolutions) on their dev servers to help trouble shoot their code.  That's why we created a [CommandBox FusionReactor](https://www.forgebox.io/view/commandbox-fusionreactor) module.  It's not part of the core, but can be installed in a single command and will attach FusionReactor's server monitor to every server you start.  You'll need to have a [FusionReactor license or sign up for a trial](http://www.fusion-reactor.com/?affiliate=ortussolutions) to use it.

```bash
install commandbox-fusionreactor
fr register "your FR license key"
server start
fr open
```

#### Web Aliases

CommandBox allows you to create web aliases for the web server that are similar to virtual directories. The alias path is relative to the web root, but can point to any folder on the hard drive. Aliases can be used for static or CFM files.  To configure aliases for your server, create an object under **web** called **alises**. The keys are the web-accessible virtual paths and the corresponding values are the relative or absolute path to the folder the alias points to.

Here's what your server.json might look like.

```javascript
{
  "web" : {
    "aliases" : {
      "/foo" : "../bar",
      "/js" : "C:\static\shared\javascript"
    }
  }
}
```

  
 Here's how to create aliases from the server set command:

```bash
server set web.aliases./foo = bar
```

#### Custom Error Pages

You can customize the error page that CommandBox servers return. You can have a setting for each status code including a default error page to be used if no other setting applies.  
 Create an errorPages object inside the web object in your server.json where each key is the status code integer or the word default and the value is a relative \(to the web root\) path to be loaded for that status code.  
 This is what you server.json might look like:

```javascript
{
  "web" : {
    "errorPages" : {
      "404" : "/path/to/404.html",
      "500" : "/path/to/500.html",
      "default" : "/path/to/default.html"
    }
  }
}
```

  
 You can set error pages via the server set command like this:

```bash
server set web.aliases.404=/missing.htm
```

#### Custom tray menu items

You can customize these tray menus and add your own option for your convenience.  To add a menu contribution to an individual server, add the following to your `server.json`:

```javascript
{
  "trayOptions":[
    {
      "label":"Foo",
      "action":"openbrowser",
      "url":"http://${Setting: runwar.host not found}:${Setting: runwar.port not found}/foobar.cfm",
      "disabled":false,
      "image":"/path/to/image.png"
    }
  ]
}
```

### Tray menu makeover

We've updated to a new library that creates the tray icon for your running servers and the menu that appears when you right click.  In addition to better support for some Linux distros, we've added some nice new icons to the menus.

![](https://www.ortussolutions.com/__media/tray.icon.3.3.0.png)

### Pre/Post package scripts

Before any package script is run, CommandBox will look for another package script with the same name, but prefixed with pre. After any package script is run, CommandBox will look for another package script with the same name, but prefixed with Post. So if you have a package that contains 3 package scripts: **foo**, **preFoo**, and **postFoo**, they will run in this order.

1. preFoo
2. foo
3. postFoo

This works for built-in package script names as well as as doc package scripts. It also works on any level. In the example above, if you created a 4th package script called **prePreFoo**, it would run prior to **preFoo**.

### Better ForgeBox Login management

If you use more than one ForgeBox login, perhaps a personal one and a company one, it can be a pain to keep logging in.  It's also hard to remember the last user you logged in with.  We've introduced two new commands to help with this.  Run this to tell you who you are logged in as:

```bash
forgebox whoami
```

Run this to switch between users that you've previously logged in with:

```bash
forgebox use myUser
```

### onRelease interceptor/package script

We've added a new "onRelease" interceptor and package script to help with the workflow of publishing packages.  Here's a run down of the three key points when bumping a package version.

* **preVersion** - Announced before the new version is set using the bump command
* **postVersion** - Announced after the new version using the bump command but before the Git repo is tagged.
* **onRelease** - Announced after a new version is set using the bump command and after the Git repo is tagged.

Here is a typical package script work flow for working with a package that's hosted on GitHub and published to ForgeBox:

```javascript
"scripts":{
  "preVersion":"testbox run",
  "postVersion":"package set location='gituser/repov#`package version`'",
  "postPublish":"!git push --follow-tags"
}
```

Then when you want to publish a new version of your package, commit your changes to Git and run the following commands:

```bash
bump --minor
publish
```

Those two commands, in combination with your package scripts, would accomplish the following:

1. Run the package's test suite \(a failure will abort the process\)
2. Increase the minor version of the page
3. Tag the Git repo
4. Change the package's location property in box.json to point to the new tag
5. Commit the tag and new box.json
6. Publish the package to ForgeBox
7. Push the new box.json and Git tag

### onInstall interceptor/package script

Announced while a package is being installed, after the package endpoint and installation directory has been resolved but before the actual installation occurs. This allows you to override things like the installation directory based on package type. Any values updated in the **interceptData** struct will override what the install command uses.

### CLI Engine Update

The Lucee version that the CLI runs on has been updated to be **4.5.3.020** which is also now the default engine to be used when you use the "**server start**" command and don't specify a **cfengine**.  If you still want to start a web server on Lucee **4.5.2.018**, then simply to this:

```bash
start cfengine=lucee@4.5.2+018
```

## Release Notes

There are tons of little bug fixes in this version that you can view in our release notes.  

### Bug

* \[[COMMANDBOX-187](https://ortussolutions.atlassian.net/browse/COMMANDBOX-187)\] - error when updating forgebox when slugname changes
* \[[COMMANDBOX-422](https://ortussolutions.atlassian.net/browse/COMMANDBOX-422)\] - Empty command CFCs with no functions throw an error starting box
* \[[COMMANDBOX-423](https://ortussolutions.atlassian.net/browse/COMMANDBOX-423)\] - Error "key \[FUNCTIONS\] doesn't exist" thrown when trying to start command box
* \[[COMMANDBOX-426](https://ortussolutions.atlassian.net/browse/COMMANDBOX-426)\] - server name completion errors on server open command
* \[[COMMANDBOX-434](https://ortussolutions.atlassian.net/browse/COMMANDBOX-434)\] - Document the resolvePath\(\) differing behaviour on OSX vs Windows
* \[[COMMANDBOX-435](https://ortussolutions.atlassian.net/browse/COMMANDBOX-435)\] - artifacts clean fails on OSX when there's .DS\_Store files
* \[[COMMANDBOX-437](https://ortussolutions.atlassian.net/browse/COMMANDBOX-437)\] - appSkeleton in the coldbox create app wizard needs to comply to IDs instead of local disk
* \[[COMMANDBOX-449](https://ortussolutions.atlassian.net/browse/COMMANDBOX-449)\] - "server open" always opens localhost
* \[[COMMANDBOX-451](https://ortussolutions.atlassian.net/browse/COMMANDBOX-451)\] - The trayicon in server.json does not work with relative paths
* \[[COMMANDBOX-464](https://ortussolutions.atlassian.net/browse/COMMANDBOX-464)\] - update command doesn't respect original install path
* \[[COMMANDBOX-465](https://ortussolutions.atlassian.net/browse/COMMANDBOX-465)\] - custom url rewrite location doesn't respect starting server by name in different location
* \[[COMMANDBOX-466](https://ortussolutions.atlassian.net/browse/COMMANDBOX-466)\] - coldbox create crud doesn't work on Windows

### New Feature

* \[[COMMANDBOX-316](https://ortussolutions.atlassian.net/browse/COMMANDBOX-316)\] - Add Fusion Reactor support for server
* \[[COMMANDBOX-399](https://ortussolutions.atlassian.net/browse/COMMANDBOX-399)\] - Starting server in web root with WEB-INF treats CWD as war
* \[[COMMANDBOX-416](https://ortussolutions.atlassian.net/browse/COMMANDBOX-416)\] - Add "open" flag to touch/new command.
* \[[COMMANDBOX-430](https://ortussolutions.atlassian.net/browse/COMMANDBOX-430)\] - forgebox whoami command to show what user your API key is set to
* \[[COMMANDBOX-431](https://ortussolutions.atlassian.net/browse/COMMANDBOX-431)\] - Update the storage of the APIkey in the commandbox settings to include multiple keys
* \[[COMMANDBOX-432](https://ortussolutions.atlassian.net/browse/COMMANDBOX-432)\] - Have a forgebox use {username} command to switch the current api key
* \[[COMMANDBOX-445](https://ortussolutions.atlassian.net/browse/COMMANDBOX-445)\] - Allow aliases \(virtual directory\) in web server
* \[[COMMANDBOX-458](https://ortussolutions.atlassian.net/browse/COMMANDBOX-458)\] - Provide custom 40x and 50x error pages for servers

### Improvement

* \[[COMMANDBOX-398](https://ortussolutions.atlassian.net/browse/COMMANDBOX-398)\] - Catch error scenario when user tries to start a server with a WEB-INF
* \[[COMMANDBOX-410](https://ortussolutions.atlassian.net/browse/COMMANDBOX-410)\] - Upgrade to latest Runwar with several bug fixes
* \[[COMMANDBOX-412](https://ortussolutions.atlassian.net/browse/COMMANDBOX-412)\] - Refactor string similarity to use external library
* \[[COMMANDBOX-413](https://ortussolutions.atlassian.net/browse/COMMANDBOX-413)\] - Refactor semver CFC to be separate lib
* \[[COMMANDBOX-414](https://ortussolutions.atlassian.net/browse/COMMANDBOX-414)\] - Refactor path pattern matcher CFC to be separate lib
* \[[COMMANDBOX-419](https://ortussolutions.atlassian.net/browse/COMMANDBOX-419)\] - coldbox create app command does not list all templates
* \[[COMMANDBOX-420](https://ortussolutions.atlassian.net/browse/COMMANDBOX-420)\] - Run pre/post package scripts by convention
* \[[COMMANDBOX-421](https://ortussolutions.atlassian.net/browse/COMMANDBOX-421)\] - Add onRelease interception point/package script
* \[[COMMANDBOX-424](https://ortussolutions.atlassian.net/browse/COMMANDBOX-424)\] - Serious performance issue with formatting large JSON strings
* \[[COMMANDBOX-425](https://ortussolutions.atlassian.net/browse/COMMANDBOX-425)\] - Better handle syntax errors in a module's config
* \[[COMMANDBOX-427](https://ortussolutions.atlassian.net/browse/COMMANDBOX-427)\] - Move onServerStart interception announcement to have server home dir
* \[[COMMANDBOX-428](https://ortussolutions.atlassian.net/browse/COMMANDBOX-428)\] - Allow tray options to be customized
* \[[COMMANDBOX-433](https://ortussolutions.atlassian.net/browse/COMMANDBOX-433)\] - Add onInstall interception point
* \[[COMMANDBOX-436](https://ortussolutions.atlassian.net/browse/COMMANDBOX-436)\] - Incorrect custom model path in the unit test
* \[[COMMANDBOX-440](https://ortussolutions.atlassian.net/browse/COMMANDBOX-440)\] - Exit REPL multi-line with extra enter stroke
* \[[COMMANDBOX-442](https://ortussolutions.atlassian.net/browse/COMMANDBOX-442)\] - Don't use in-use port specified in start params or server.json
* \[[COMMANDBOX-443](https://ortussolutions.atlassian.net/browse/COMMANDBOX-443)\] - Wait for full debug output when starting server with debug=true
* \[[COMMANDBOX-444](https://ortussolutions.atlassian.net/browse/COMMANDBOX-444)\] - Append JVM args and runwar args to server defaults
* \[[COMMANDBOX-454](https://ortussolutions.atlassian.net/browse/COMMANDBOX-454)\] - Update to Runwar 3.4.10
* \[[COMMANDBOX-457](https://ortussolutions.atlassian.net/browse/COMMANDBOX-457)\] - Improve server status detections
* \[[COMMANDBOX-460](https://ortussolutions.atlassian.net/browse/COMMANDBOX-460)\] - Show tag stack when executing .cfm files
* \[[COMMANDBOX-461](https://ortussolutions.atlassian.net/browse/COMMANDBOX-461)\] - Improve tab completion of forgebox slugs
* \[[COMMANDBOX-467](https://ortussolutions.atlassian.net/browse/COMMANDBOX-467)\] - Allow custom images inside tray menu plus disabled items
* \[[COMMANDBOX-468](https://ortussolutions.atlassian.net/browse/COMMANDBOX-468)\] - Add overwrite confirmations to all coldbox create commands
* \[[COMMANDBOX-469](https://ortussolutions.atlassian.net/browse/COMMANDBOX-469)\] - Improve version output in "forgebox show" command
* \[[COMMANDBOX-470](https://ortussolutions.atlassian.net/browse/COMMANDBOX-470)\] - Upgrade engine to Lucee 4.5.3.020

