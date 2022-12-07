# What's New in 3.5.0

### CLI Shell

We upgraded to a new version of the underlying library that handles the CLI interactions which brought a number of nice things.

* The **cls** command now clears background colors that used to be left on the screen
* Tab completion works better when two folders with different case were in the same directory
* When developing commands, default text can be placed in the buffer when asking the user for input.
* Fixed some instances where undesired spaces would get added when hitting tab completion

We also made the following improvements to the CLI shell environment

* You don't need to escape an equals sign that's part of a quoted parameter value
* Removed extra line break when piping the output of the "run" command
* Fixed some regressions when piping text to the box binary
* Added better BOM detection when tailing files
* **cp** command will create destination directories
* Windows paths that start with \ or / will be treated as absolute (like DOS works)
* All OS's will expand \~ to the current user's home directory

### Tail Command

The tail command used to only take a file as input, but now you can pipe raw text in as well.

```bash
CommandBox> forgebox search | tail lines=50
```

When tailing a file, you can specify the **--follow** flag and any new text added to the file will live stream to your console until you press Ctrl-C to stop.  This is perfect for tailing application logs while your code is running to see new entries.

```bash
CommandBox> tail myFile.log --follow
CommandBox> system-log | tail --follow
```

The **server log** command also has a new **--follow** flag added to it which will live stream a running server's console log to the shell until you press Ctrl-C to stop it.

```bash
CommandBox> server log --follow
```

### Package Management

The artifact storage location is now customizable thanks to Chris Schmitz, allowing you to store your artifacts on another drive, or even a network share so your coworkers can all use the same "local" copies.

```bash
CommandBox> config set artifactsDirectory=/path/to/artifacts
```

CommandBox will always re-download snapshot versions of packages to make sure you get a fresh version. &#x20;

```bash
CommandBox> install myPackage@1.2.3-snapshot
```

When you try to install a package and CommandBox is offline, instead of giving up, we'll now look in your local artifacts cache for a satisfying version.  If we find a package that works in your artifacts, we'll install it instead. &#x20;

### Server Starting

We added a few new ways to start up a server.  You can use the **--debug** flag when starting a server to see additional information and the foreground process also waits for the server to start before finishing.  Now when starting in debug mode this output will stream to the console as it becomes available instead of showing up all at the end.  This is great to troubleshoot errors that are happening on server start as well as finding the slow parts of the startup sequence.

```bash
CommandBox> server start --debug
```

By default, your servers fire up in a new process that runs independently from the CLI.  There is now a new flag called **--console** that will start the server up in the foreground and stream the console output to the CLI for you to watch.   The start command will not end and will keep streaming the console output until you press Ctrl-C to stop it.  You can also use **--debug** alongside the console flag for even more information.

```bash
CommandBox> server start --console
CommandBox> server start --console --debug
```

### Server Welcome Files

If you have an app that uses a default welcome file other than **index.cfm**, you can control that now.

```bash
CommandBox> server set web.welcomeFiles="go.cfm,main.cfm,index.cfm,index.html"
```

### Better SES URLs

This is one that you take for granted if you've always used Adobe ColdFusion, but for any CF server not running on Adobe's custom version of Tomcat, you can't use SES URLs in a subfolder like this without adding custom mappings to your **web.xml**:

```
site.com/myFolder/index.cfm/home/login
```

We've added just a dash of servlet fairy dust that now makes this possible.  Note, if you want to hide the **index.cfm** with URL rewrites, you'll need a [custom rewrite config](https://ortus.gitbooks.io/commandbox-documentation/content/embedded\_server/url\_rewrites.html) for it to work in a subfolder.

### Server Configuration

All server engines and versions have been standardized to install into the same reliable directory structure to make it easier for you to script config file replacements. &#x20;

* For **Adobe CF WARs,** the xml config files are located in the WAR here: **/WEB-INF/cfusion/lib/neo.\*.xml**
* For the **Lucee server context**, the xml config file is located in the WAR here: **/WEB-INF/lucee-server/context/lucee-server.xml**
* For the **Lucee web context**, the xml config file is located in the WAR here: **/WEB-INF/lucee-web/lucee-web.xml.cfm**

To find the folder where your WEB-INF lives (as well as lots of other information about your server) you can use the **server info** command to get useful properties about a starting or started server.

```bash
# Find the "home" directory for a server (where the WEB-INF lives)
CommandBox> server info property=serverHomeDirectory

# Find the out log file
CommandBox> server info property=consoleLogPath

# Get all possible values as JSON
CommandBox> server info --JSON
```

Combining these allows you to do some nice one-liners like scripting out the copying of config settings when the server starts up.  Hint: [Use an **onServerInstall** or **onServerStart** package script](https://commandbox.ortusbooks.com/content/v/development/embedded\_server/copy-configs.html)!

```bash
CommandBox> cp neo-datasource.xml '`server info property=serverHomeDirectory`/WEB-INF/cfusion/lib/neo-datasource.xml'
```

Read more about this here:

[https://commandbox.ortusbooks.com/content/v/development/embedded\_server/copy-configs.html](https://commandbox.ortusbooks.com/content/v/development/embedded\_server/copy-configs.html)

### Custom Server Home

Until now you've had to live with the special directory that CommandBox uses to install your servers into.  Now you can get full control over where the server goes which is perfect for creating a folder "seeded" with config files that you want the server to use when it first starts.  This trick (with some [clever Git ignores](https://commandbox.ortusbooks.com/content/v/development/embedded\_server/custom-server-home.html)) will also allow you to commit changes to your config files back to the repo while ignoring the rest of the engine.

* Customize Lucee's **server** context folder with the **serverConfigDir** setting
* Customize Lucee's **web** context folder with the **webConfigDir** setting
* Customize where the entire WAR explodes to for any server with the **serverHomeDirectory** setting

This is very powerful since it gives you full control over the server deployment.  Server installs have also been changed to NOT overwrite existing files when they unzip, so any config files you place in the server home prior to starting the server will be left in place and used by the server when it starts up.  This means you can have datasources, mappings, and more start out-of-the-box for your site on a fresh install.

Read more about this here.

[https://commandbox.ortusbooks.com/content/v/development/embedded\_server/custom-server-home.html](https://commandbox.ortusbooks.com/content/v/development/embedded\_server/custom-server-home.html)

### A Few Changes

There were a few small changes in the "undocumented" core of CommandBox that got rearranged to make more sense.  There's a small chance you may have been relying on one of them, so take note:

* The **serverHome** property that comes back from **server info** has been renamed to **serverHomeDirectory**.
* The **webConfigDir** property used to point to the server home, but this was incorrect.  The property will now be blank unless specified.  Use **serverHomeDirectory** instead.
* The default Lucee server used to have a non-standard folder structure, but now matches the WAR folders of all other servers
* The web context in Lucee servers used to be in a folder named after a random hash which was kind of silly (and impossible to find).  It's now always under **/WEB-INF/lucee-web**
* All "internal" Lucee servers used to share a single server context (and settings).  All servers are now separate.  Use the **serverConfigDir** setting to point more than one server at a single server context or use one of the new options for copying configs.
* The core CLI server context now has a default password of "commandbox" set.  This would apply if you wanted to use the tag from .cfm files executed via the shell or a custom command.
* Several new properties were added to the **server info** data for your convenience.  Check them out by starting a server and running **server info --JSON**.



## Release Notes

### Bug

* \[[COMMANDBOX-236](https://ortussolutions.atlassian.net/browse/COMMANDBOX-236)] - \`box reload\` doesn't clear background colors from buffer on Windows
* \[[COMMANDBOX-248](https://ortussolutions.atlassian.net/browse/COMMANDBOX-248)] - tab completion doesn't always work on paths
* \[[COMMANDBOX-500](https://ortussolutions.atlassian.net/browse/COMMANDBOX-500)] - CommandBox timeout is shorter than runwar timeout when starting Adobe servers
* \[[COMMANDBOX-505](https://ortussolutions.atlassian.net/browse/COMMANDBOX-505)] - BOM interferes with commandbox.properties
* \[[COMMANDBOX-507](https://ortussolutions.atlassian.net/browse/COMMANDBOX-507)] - Staring server with defautlPort in box.json, adds optional keys back in.
* \[[COMMANDBOX-509](https://ortussolutions.atlassian.net/browse/COMMANDBOX-509)] - Ignore equals in a quoted parameter
* \[[COMMANDBOX-522](https://ortussolutions.atlassian.net/browse/COMMANDBOX-522)] - Improve error message when endpoint fails installing server
* \[[COMMANDBOX-526](https://ortussolutions.atlassian.net/browse/COMMANDBOX-526)] - Use hostname for "coldbox reinit"
* \[[COMMANDBOX-533](https://ortussolutions.atlassian.net/browse/COMMANDBOX-533)] - Error starting CommandBox in some instances
* \[[COMMANDBOX-539](https://ortussolutions.atlassian.net/browse/COMMANDBOX-539)] - "run" expressions contain line break on Linux
* \[[COMMANDBOX-542](https://ortussolutions.atlassian.net/browse/COMMANDBOX-542)] - Regression in piping input from OS console
* \[[COMMANDBOX-543](https://ortussolutions.atlassian.net/browse/COMMANDBOX-543)] - Piping a file of commands with a BOM into box fails
* \[[COMMANDBOX-544](https://ortussolutions.atlassian.net/browse/COMMANDBOX-544)] - package set doesn't always set what you expect
* \[[COMMANDBOX-545](https://ortussolutions.atlassian.net/browse/COMMANDBOX-545)] - The \`commandbox-home\` when used in symbolic link mode fails on mac

### New Feature

* \[[COMMANDBOX-234](https://ortussolutions.atlassian.net/browse/COMMANDBOX-234)] - ability for server start to deploy web-inf locally instead of server location
* \[[COMMANDBOX-473](https://ortussolutions.atlassian.net/browse/COMMANDBOX-473)] - Control list of welcome files
* \[[COMMANDBOX-479](https://ortussolutions.atlassian.net/browse/COMMANDBOX-479)] - Make artifacts path customizable
* \[[COMMANDBOX-503](https://ortussolutions.atlassian.net/browse/COMMANDBOX-503)] - Allow tail command to follow a log file
* \[[COMMANDBOX-504](https://ortussolutions.atlassian.net/browse/COMMANDBOX-504)] - Allow raw text to be piped into the tail command
* \[[COMMANDBOX-506](https://ortussolutions.atlassian.net/browse/COMMANDBOX-506)] - Add startTimeout parameter to control how long to wait for server to start
* \[[COMMANDBOX-508](https://ortussolutions.atlassian.net/browse/COMMANDBOX-508)] - Console flag to server start
* \[[COMMANDBOX-523](https://ortussolutions.atlassian.net/browse/COMMANDBOX-523)] - new preServerStart interceptor
* \[[COMMANDBOX-534](https://ortussolutions.atlassian.net/browse/COMMANDBOX-534)] - Add --follow flag to "server log" to tail it and follow
* \[[COMMANDBOX-535](https://ortussolutions.atlassian.net/browse/COMMANDBOX-535)] - cp command create directories if necessary when copying file
* \[[COMMANDBOX-536](https://ortussolutions.atlassian.net/browse/COMMANDBOX-536)] - If Forgebox is down, use artifacts cache on installs
* \[[COMMANDBOX-538](https://ortussolutions.atlassian.net/browse/COMMANDBOX-538)] - Allow programmatic access to server info
* \[[COMMANDBOX-546](https://ortussolutions.atlassian.net/browse/COMMANDBOX-546)] - Allow custom server home dir

### Improvement

* \[[COMMANDBOX-153](https://ortussolutions.atlassian.net/browse/COMMANDBOX-153)] - Support for double wildcard servlet mappings
* \[[COMMANDBOX-222](https://ortussolutions.atlassian.net/browse/COMMANDBOX-222)] - Set default password Lucee CLI context
* \[[COMMANDBOX-439](https://ortussolutions.atlassian.net/browse/COMMANDBOX-439)] - absolute paths on Windows don't follow the same rules as DOS
* \[[COMMANDBOX-456](https://ortussolutions.atlassian.net/browse/COMMANDBOX-456)] - If forgebox is down, 'internal' server won't start
* \[[COMMANDBOX-498](https://ortussolutions.atlassian.net/browse/COMMANDBOX-498)] - Upgrade engine to Lucee 4.5.4.017
* \[[COMMANDBOX-499](https://ortussolutions.atlassian.net/browse/COMMANDBOX-499)] - improve contentbox-widget package installation conventions
* \[[COMMANDBOX-501](https://ortussolutions.atlassian.net/browse/COMMANDBOX-501)] - Stream server start log when debug is true
* \[[COMMANDBOX-510](https://ortussolutions.atlassian.net/browse/COMMANDBOX-510)] - Improve JSON parsing when piping complex values to cfml command
* \[[COMMANDBOX-511](https://ortussolutions.atlassian.net/browse/COMMANDBOX-511)] - Allow webConfigDir, serverConfigDir & webXML to be relative
* \[[COMMANDBOX-514](https://ortussolutions.atlassian.net/browse/COMMANDBOX-514)] - Improve rewrites to not fire on SES URLs in a subdir
* \[[COMMANDBOX-515](https://ortussolutions.atlassian.net/browse/COMMANDBOX-515)] - server forget does not stop server if running
* \[[COMMANDBOX-518](https://ortussolutions.atlassian.net/browse/COMMANDBOX-518)] - libdirs aren't relative when starting a server
* \[[COMMANDBOX-519](https://ortussolutions.atlassian.net/browse/COMMANDBOX-519)] - Libdirs aren't used for non-internal servers.
* \[[COMMANDBOX-520](https://ortussolutions.atlassian.net/browse/COMMANDBOX-520)] - Improve output of server info and server list commands
* \[[COMMANDBOX-521](https://ortussolutions.atlassian.net/browse/COMMANDBOX-521)] - Stop loading java agent for Lucee 5
* \[[COMMANDBOX-524](https://ortussolutions.atlassian.net/browse/COMMANDBOX-524)] - Improve starting internal server when not specifying buildID
* \[[COMMANDBOX-528](https://ortussolutions.atlassian.net/browse/COMMANDBOX-528)] - Improve server start intercepors
* \[[COMMANDBOX-529](https://ortussolutions.atlassian.net/browse/COMMANDBOX-529)] - Standardize server home directories
* \[[COMMANDBOX-530](https://ortussolutions.atlassian.net/browse/COMMANDBOX-530)] - Upgrade to JLine 2.15-snapshot
* \[[COMMANDBOX-531](https://ortussolutions.atlassian.net/browse/COMMANDBOX-531)] - Allow default text to be put in buffer for ask() function
* \[[COMMANDBOX-532](https://ortussolutions.atlassian.net/browse/COMMANDBOX-532)] - Don't cache snapshots
* \[[COMMANDBOX-537](https://ortussolutions.atlassian.net/browse/COMMANDBOX-537)] - Remove .git folder when cloning a Git repo
* \[[COMMANDBOX-540](https://ortussolutions.atlassian.net/browse/COMMANDBOX-540)] - Support \~ as a shortcut for the user home directory like bash.
* \[[COMMANDBOX-547](https://ortussolutions.atlassian.net/browse/COMMANDBOX-547)] - Improve tab completion for server/package/config set commands
* \[[COMMANDBOX-549](https://ortussolutions.atlassian.net/browse/COMMANDBOX-549)] - Spruce up the opening ASCII art
* \[[COMMANDBOX-550](https://ortussolutions.atlassian.net/browse/COMMANDBOX-550)] - Pass JVM args through to background server process
* \[[COMMANDBOX-551](https://ortussolutions.atlassian.net/browse/COMMANDBOX-551)] - Fix working directory of xxxInstall package scripts
