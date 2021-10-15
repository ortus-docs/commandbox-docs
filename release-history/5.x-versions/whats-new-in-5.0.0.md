# What's New in 5.0.0

## Upgrade Compatibility

Even though this is a new major version, it should be very backwards compatible.  CommandBox proper has no known backwards compatibility issues we're aware of, but note we've bumped libraries like Lucee Server, Undertow, and Java support, so you may notice differences due to these 3rd party lib updates.  For example, one [breaking change](https://dev.lucee.org/t/cfdump-not-rendering-when-output-false/3589) in Lucee 5.3 (which now powers your default server) is how page output buffer is handled. 

You should be able to simply replace your box.exe binary and run it to get the same in-place upgrade you're used to.  If you run into issue, you can try removing the "engine" folder in your \~/.CommandBox folder and try again.  If you need to downgrade for any reason, replace the box binary with the old version, remove the "engine", "cfml", and "lib" folders in your CommandBox home and they will get re-created on the next run.

You may also notice the box binary is larger now.  From 44 Megs up to 77 Megs.  This is a regrettable byproduct of us turning off the Pack200 process we used to run against the Lucee jar.  Lucee seems to have some bugs that causes it to re-download a bunch of OSGI bundles when we compress them and we can't figure it out, or get support on the matter, so for now we're just not compressing as much stuff.  This was a huge blocker for anyone needing to run CommandBox on a PC with no external internet access as Lucee provides no mechanism to turn off it's auto-download behavior.

And finally, if you have installed any custom OSGI bundles into your CLI, they will be wiped by the upgrade process now.  We didn't want to have to to do this, but Lucee has bugs that cause errors when doing in-place upgrades with the old OSGI bundles left in place and we couldn't get it fixed, so this was the only way to ensure in-place upgrades would "just work" without errors.  We are leaving the Lucee engine folder, so any settings you may have put into your CLI should remain in place.

## What's New?

There's a lot of new stuff in CommandBox 5.0.0.  Here's an overview.   One of the biggest new "features" is you can finally use CommandBox on Java 11+.  This was not possible in CommandBox 4.x due to the version of Lucee not fully supporting newer versions of Java.  Now that Lucee has been bumped to 5.3 (see below) you are free to leave java 8 behind for the CLI.  Note if you're using CommandBox to start up older versions of Adobe CF or Lucee/Railo, you may still need to use java 8 specifically for your servers.  

### New Libraries

Let's start with the library updates.  For the most part, all the jars we bundle are a "black box" but in reality, every update is usually for new features, fixes, or security patches.  Here's an overview of the new libs:

* **WireBox** 5.6.2
* **JLine** 3.13.0
* **Runwar** 4.0.3 _(major bump from 3.x)_
* **JBoss Undertow** 2.0.27.Final _(major bump from 1.x)_
* **JGit **5.5.1.201910021850-r
* **Lucee **5.3.4.77_("major" bump from 5.2)_
* **AdoptOpenJDK **jdk-11.0.6+10 (In the JRE-included download) _(major bump from 8.x)_

### New Features/Enhancements

You can now use user/pass or personal access token authentication when cloning Git repos over HTTPS.  This has been tested with Github and Gitlab and is an alternative to SSH keys.  Please check the docs, as Github and Gitlab both expect slightly different inputs.

```bash
install git+https://username:password@domain.com/user/repo.git
or
install git+https://AccessTokenHere@github.com/user/repo.git
```

There is a new Lex installation endpoint to help you acquire Lucee Extensions your app needs via the "install" command or a dependency in your box.json file.  If the current directory has a Lucee server in it, CommandBox will install the extension file directly into your server's "deploy" folder (server context)

```bash
install lex:https://downloads.ortussolutions.com/ortussolutions/lucee-extensions/ortus-redis-cache/1.4.0/ortus-redis-cache-1.4.0.lex
```

The auto-install feature into your server will work on any Lex package, even one coming from ForgeBox:

```
// ForgeBox slug for Ortus Redis Extension
install 5C558CC6-1E67-4776-96A60F9726D580F1
```

Tuning your server is easier now.  You can configure your Undertow worker-threads setting with first-class server.json property

```bash
server set web.maxRequests=200
```

Which gives you this in your server.json

```javascript
{
  "web" : {
    "maxRequests" : 200
  }
}
```

We've also unlocked a method for you to set ANY valid Undertow option or XNIO option.  So if Undertow supports it, you can configure it!

```bash
server set runwar.undertowOptions.ALLOW_UNESCAPED_CHARACTERS_IN_URL=true

server set runwar.XNIOOptions.WORKER_NAME=myWorker
```

We've added a new experimental feature that lets you create a batch file, powershell script, or bash shell script that directly starts a server with the exact settings that you get from "server start".  This is for you to create super-optimzed startups in Docker or Service that bypass the CLI steps and "lock in" the settings.  No server.json or CFConfig, or dotenv code will be processed, but you will have a fast streamlined start that is the same every time.  Couple this with our new "dryRun" flag on server start that will unpack the CF engine, but not actually start the server, and you can create your customs start script like so:

```bash
server start --console --dryRun startScript=bash startScriptFile=startmebaby.sh

// Later directly from bash...

./startmebaby.sh
```

The Globber helper can now take more than one globbing pattern.  This also means every built-in command in CommandBox that takes a globbing pattern, can now take a comma delimited list of patterns.  We've also added an exclude list of globbing patterns to the dir command as well.

```bash
dir **.cfc,*.cfm
dir paths=modules excludePath=**.md --recurse
dir paths=samples sort="directory asc, name desc"
```

We've got a couple new handy commands to help you from the command line, "unique" (modeled after the Bash "uniq" command)

```bash
cat names.txt | unique
cat names.txt | unique --count
```

And "sort" (modeled after the Bash "sort" command)

```bash
cat names.txt | sort
cat names.txt | sort type=text
cat names.txt | sort type=numeric
cat names.txt | sort direction=desc
```

The "grep" command has received a "count" parameter if you just want the count of lines that match the regex (or no regex will count all lines)

```bash
dir **.cfc | grep --count

dir | grep .*\.md --count
```

The tray icon for your servers now has a new option under the "Open" menu that will open up the file system folder where the server home lives.  This is nice for finding your CF Engine's log files.

![](https://www.ortussolutions.com/\__media/blog/new-menu-commandbox-5-0-0.png)

## Release Notes

There are a lot of bug fixes and even more enhancements I didn't cover above.  You can read the full release notes here:

### Sub-task

* \[[COMMANDBOX-1069](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1069)] - Remove extra stashes on url paths when servlet init params starts with WEB-INF

### Bug

* \[[COMMANDBOX-643](https://ortussolutions.atlassian.net/browse/COMMANDBOX-643)] - Tray Icon not displaying on Debian8
* \[[COMMANDBOX-711](https://ortussolutions.atlassian.net/browse/COMMANDBOX-711)] - X Window Errors
* \[[COMMANDBOX-812](https://ortussolutions.atlassian.net/browse/COMMANDBOX-812)] - "Coldbox create resource" uses wrong paths on Windows
* \[[COMMANDBOX-941](https://ortussolutions.atlassian.net/browse/COMMANDBOX-941)] - urlrewrite.xml has file size of 0 on docker restart but not regular start
* \[[COMMANDBOX-946](https://ortussolutions.atlassian.net/browse/COMMANDBOX-946)] - CommandBox instances crashing because of TrayIcon rendering
* \[[COMMANDBOX-975](https://ortussolutions.atlassian.net/browse/COMMANDBOX-975)] - CommandBox always reads STDIN even when in non-interactive mode
* \[[COMMANDBOX-980](https://ortussolutions.atlassian.net/browse/COMMANDBOX-980)] - Using zsh exits out of CommandBox when running a binary command
* \[[COMMANDBOX-992](https://ortussolutions.atlassian.net/browse/COMMANDBOX-992)] - Shebang scripts no longer work without .cfm extension
* \[[COMMANDBOX-1005](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1005)] - If custom rewrite file is already in correct destination, runwar overwrites it as 0 bytes
* \[[COMMANDBOX-1008](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1008)] - worker-threads setting no longer has any affect
* \[[COMMANDBOX-1009](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1009)] - Tray icon not showing in Ubuntu 18.04
* \[[COMMANDBOX-1013](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1013)] - Undertow error output when starting server
* \[[COMMANDBOX-1014](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1014)] - \[RUNWAR] Tray menu placeholders such as ${Setting: runwar.port not found} are not replaced in sub menus
* \[[COMMANDBOX-1022](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1022)] - regex string index out of bounds exception
* \[[COMMANDBOX-1035](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1035)] - URL Rewrites fire incorrect on URL containing a space
* \[[COMMANDBOX-1036](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1036)] - Browser doesn't open when server start
* \[[COMMANDBOX-1037](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1037)] - Host updater does not work
* \[[COMMANDBOX-1038](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1038)] - Server doesn't stop on Windows
* \[[COMMANDBOX-1043](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1043)] - ConcurrentModificationException with undertow?
* \[[COMMANDBOX-1048](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1048)] - Restrict /dumprunwarrequest to be used only on Unit Testing
* \[[COMMANDBOX-1050](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1050)] - Remove trailing slash from Adobe updates path
* \[[COMMANDBOX-1054](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1054)] - Default command parameters don't work on commands in namespaces
* \[[COMMANDBOX-1061](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1061)] - Command DSL has unexpected behavior with equals sign in positional tokens
* \[[COMMANDBOX-1062](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1062)] - Tab complete for negated flags isn't complete
* \[[COMMANDBOX-1063](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1063)] - box fails to open in vSphere Web Client console due to outdated JLine jar
* \[[COMMANDBOX-1068](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1068)] - Server start doesn't correctly expand relative Java Home directory
* \[[COMMANDBOX-1070](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1070)] - slashes into the servlet init param paths
* \[[COMMANDBOX-1071](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1071)] - Tray icon doesn't disappear on Windows when server stops from CLI
* \[[COMMANDBOX-1076](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1076)] - Install dependency from box.json with env var placeholder gets overwritten with actual value
* \[[COMMANDBOX-1077](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1077)] - coldbox create resource command ignores specsDirectory argument
* \[[COMMANDBOX-1079](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1079)] - foreach cannot be interrupted with Ctrl-C
* \[[COMMANDBOX-1081](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1081)] - validate non-numeric exit codes from the "exit" command
* \[[COMMANDBOX-1089](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1089)] - Task Runner's loadModule() fails on path with period
* \[[COMMANDBOX-1090](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1090)] - Tasks don't treat return 1 and setExitCode( 1 ) the same
* \[[COMMANDBOX-1091](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1091)] - When a task sets a failing exit code, no output is sent to console
* \[[COMMANDBOX-1092](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1092)] - Commands that set a failing exit code don't raise proper exception
* \[[COMMANDBOX-1098](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1098)] - foreach, grep, and sed only break on chr(10)

### New Feature

* \[[COMMANDBOX-148](https://ortussolutions.atlassian.net/browse/COMMANDBOX-148)] - Ability to install/uninstall box server services
* \[[COMMANDBOX-715](https://ortussolutions.atlassian.net/browse/COMMANDBOX-715)] - Server command to explode server war but not start it
* \[[COMMANDBOX-1007](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1007)] - Set any valid XNIO option
* \[[COMMANDBOX-1018](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1018)] - Add generic feature to set any valid Undertow option
* \[[COMMANDBOX-1028](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1028)] - Allow no rest mappings to be supplied
* \[[COMMANDBOX-1032](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1032)] - Tab completion for task targets
* \[[COMMANDBOX-1052](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1052)] - New TestBox commands: generate visualizer and generate browser
* \[[COMMANDBOX-1053](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1053)] - Update ColdBox module templates for 5.0 standards
* \[[COMMANDBOX-1067](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1067)] - Expose Undertow worker-threads setting with first-class server.json property
* \[[COMMANDBOX-1075](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1075)] - Support HTTPS username/password auth
* \[[COMMANDBOX-1083](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1083)] - Option for server start to write file with full start args for direct Runwar call
* \[[COMMANDBOX-1088](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1088)] - Add lex endpoint for installing Lucee extensions
* \[[COMMANDBOX-1096](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1096)] - Add unique command to filter out duplicates rows of input
* \[[COMMANDBOX-1097](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1097)] - Add sort command to sort rows of input

### Task

* \[[COMMANDBOX-964](https://ortussolutions.atlassian.net/browse/COMMANDBOX-964)] - Make sure the build works
* \[[COMMANDBOX-965](https://ortussolutions.atlassian.net/browse/COMMANDBOX-965)] - Document Setup in the Repo
* \[[COMMANDBOX-995](https://ortussolutions.atlassian.net/browse/COMMANDBOX-995)] - Vet all changes on Runwar since April 25, 2018.

### Improvement

* \[[COMMANDBOX-885](https://ortussolutions.atlassian.net/browse/COMMANDBOX-885)] - Enhance Globber to take more than one pattern
* \[[COMMANDBOX-886](https://ortussolutions.atlassian.net/browse/COMMANDBOX-886)] - Enhance Globber to have exclude patterns
* \[[COMMANDBOX-963](https://ortussolutions.atlassian.net/browse/COMMANDBOX-963)] - Update CLI to Lucee 5.3 and test Java 11
* \[[COMMANDBOX-1029](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1029)] - Change CommandBox build to pull Ortus build of Runwar
* \[[COMMANDBOX-1049](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1049)] - Output Runwar version and jar path in "info" command output
* \[[COMMANDBOX-1055](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1055)] - Need Config for Max Thread Request at runwar
* \[[COMMANDBOX-1057](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1057)] - Enhance dir command with new Globber features
* \[[COMMANDBOX-1058](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1058)] - Optimize installation of packages with createPackageDirectory set to false
* \[[COMMANDBOX-1059](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1059)] - Improve performance of print buffer by using String Builder internally
* \[[COMMANDBOX-1060](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1060)] - Add --count flag to grep command
* \[[COMMANDBOX-1065](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1065)] - Update CommandBox to Runwar 4.0.0
* \[[COMMANDBOX-1066](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1066)] - Upgrade to JGit 5.5
* \[[COMMANDBOX-1074](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1074)] - Tie into FusionReactor to report transactions for tasks, commands, etc.
* \[[COMMANDBOX-1086](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1086)] - Default location to forgeboxStorage for package init command
* \[[COMMANDBOX-1087](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1087)] - Improve default package naming of jar endpoint
* \[[COMMANDBOX-1093](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1093)] - Add option to tray menu to open server home directory
* \[[COMMANDBOX-1095](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1095)] - Update to latest WireBox
* \[[COMMANDBOX-1099](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1099)] - Stop outputting extra line break for commands with no output
