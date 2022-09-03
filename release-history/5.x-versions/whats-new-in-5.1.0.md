# What's New in 5.1.0

### &#x20;Java 14 support

Java 14 is now supported in CommandBox 5.1.0.  In order to support Java 14, we had to stop using Pack200 which means the binary sizes have grown a little.  The good news is CommandBox will start up a little faster on its first run since there's less to unpack now.

### Start pure HTML Server

You can start up a lightweight server that only serves static files now with CommandBox. &#x20;

```bash
server start cfengine=none
```

[https://commandbox.ortusbooks.com/v/5.1.0/embedded-server/start-html-server](https://commandbox.ortusbooks.com/v/5.1.0/embedded-server/start-html-server)

### New CommandBox Light and CommandBox Thin Binaries

In pursuit of the smallest possible Docker images, we have CommandBox light which is built on Lucee Light.  We also have a box "thin" binary you can swap out with the full self-extracting binary when using CommandBox in custom docker images.  Check out Pete Freitag's [Minibox image](https://www.petefreitag.com/item/899.cfm) to see both of these in use in a super tiny 78 Meg docker image.  More docs here:

[https://commandbox.ortusbooks.com/v/5.1.0/setup/light-and-thin-binaries](https://commandbox.ortusbooks.com/v/5.1.0/setup/light-and-thin-binaries)

### Force working directory when starting

If you're using box in an integration where you want it to start up in a specific working directory, there is a new bootstrap CLI arg for that.

```bash
box -cliworkingDir=C:/my/path/here/
```

[https://commandbox.ortusbooks.com/v/5.1.0/usage/execution#custom-working-directory](https://commandbox.ortusbooks.com/v/5.1.0/usage/execution#custom-working-directory)

### Server tray menu item custom commands

You've always been able to specify custom menu items in your server.json or global config settings, but we've kicked it up a notch.  Not only can you contribute to existing sub menus now, you can execute arbitrary native commands synchronously or async.

```javascript
{
    "trayOptions" : [
        {
            "label" : "Does the Internet work?",
            "action" : "run",
            "command" : "ping google.com"
        },
        {
            "label" : "Math is math!",
            "action" : "runAsync",
            "command" : "calc.exe"
        },
        {
            "label" : "Update dependencies",
            "action" : "runTerminal",
            "command" : "box update"
        }
    ]
}
```

## Release Notes

Here's the full list of tickets closed down in the 5.1.0 release.

## Bug

* \[[COMMANDBOX-1121](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1121)] - Runwar deadlocks when using Lucee server warmup flag
* \[[COMMANDBOX-1122](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1122)] - Server start console output isn't always formatted correctly
* \[[COMMANDBOX-1125](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1125)] - Boolean env var causes error on server start
* \[[COMMANDBOX-1127](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1127)] - Output of foreach can't be piped
* \[[COMMANDBOX-1133](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1133)] - Lucee Extension install doesn't recognize Lucee Light
* \[[COMMANDBOX-1135](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1135)] - Package unlink command misspelled parameter moduleDirectory as moduleDrectory
* \[[COMMANDBOX-1137](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1137)] - Package link command misspelled parameter moduleDirectory as moduleDrectory
* \[[COMMANDBOX-1140](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1140)] - Tab complete doesn't work on Windows paths with backslashes
* \[[COMMANDBOX-1144](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1144)] - CommandBox Watcher shows error on Ctrl-C
* \[[COMMANDBOX-1148](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1148)] - Extension management doesn't "recognize" a Lucee server started with --dryRun
* \[[COMMANDBOX-1151](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1151)] - Downgrading a package with install doesn't work without --force
* \[[COMMANDBOX-1152](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1152)] - The run command doesn't always seem to kill interactive binaries
* \[[COMMANDBOX-1154](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1154)] - Relative paths incorrect in drive root on \*nix
* \[[COMMANDBOX-1166](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1166)] - Using a warPath of ./ gets normalized to "" and then ignored in subsequent starts
* \[[COMMANDBOX-1176](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1176)] - native commands with \* can fail due to missing regex escape
* \[[COMMANDBOX-1177](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1177)] - Incorrect serverInfo for a server that hasn't started

### New Feature

* \[[COMMANDBOX-1015](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1015)] - Allow arbitrary actions for menu items
* \[[COMMANDBOX-1019](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1019)] - Allow to start a pure HTML server
* \[[COMMANDBOX-1130](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1130)] - Update ColdBox Templates to new standards
* \[[COMMANDBOX-1145](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1145)] - Allow default working dir of box to be overridden
* \[[COMMANDBOX-1146](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1146)] - Create box-thin binaries that don't bundle any libs
* \[[COMMANDBOX-1147](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1147)] - Create CommandBox Light built that uses Lucee Light jar
* \[[COMMANDBOX-1153](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1153)] - Add two new methods to commands for working with async futures: getCurrentThread() getThreadName()
* \[[COMMANDBOX-1170](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1170)] - TestBox run commands now support the outputFormats argument to allow you to output post-test reports in many formats
* \[[COMMANDBOX-1171](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1171)] - TestBox revamped UI for the CLI reporter
* \[[COMMANDBOX-1172](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1172)] - Add Java info debug to box binary

### Task

* \[[COMMANDBOX-1102](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1102)] - Add Java version as info log

### Improvement

* \[[COMMANDBOX-1080](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1080)] - Provide way to disable server instance icon in MacOS dock
* \[[COMMANDBOX-1126](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1126)] - Add --full flag to dir command to output full path
* \[[COMMANDBOX-1129](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1129)] - Rework the server list command so it is more performant
* \[[COMMANDBOX-1132](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1132)] - Bump to Lucee 5.3.6.61
* \[[COMMANDBOX-1134](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1134)] - Tab complete for sort options in dir command
* \[[COMMANDBOX-1143](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1143)] - Have the ProgressableDownloader send an Accept header
* \[[COMMANDBOX-1150](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1150)] - Don't overwrite lucee-server.xml file when updating libs
* \[[COMMANDBOX-1155](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1155)] - Auto-detect \*unix distros with non-bash shells
* \[[COMMANDBOX-1156](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1156)] - Copy lco files so Lucee server can start on CommandBox Light
* \[[COMMANDBOX-1168](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1168)] - Reset console window title after \`run\` executes a process
