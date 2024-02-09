# What's New in 5.8.0

### Bundled System Modules

To be more useful, CommandBox now bundles the following system modules

* [commandbox-cfconfig](https://www.forgebox.io/view/commandbox-cfconfig)
* [commandbox-dotenv](https://www.forgebox.io/view/commandbox-dotenv)
* [commandbox-update-check](https://www.forgebox.io/view/commandbox-update-check)

They will be automatically installed (or updated) when you start the CLI for the first time.  You can still update or uninstall them, just like any system module.  Note:  If you have any of these modules currently linked into the CommandBox core, any uncommitted changes will be overwritten when you upgrade box.  Please unlink the repos first before upgrading.

The CommandBox Update Check modules can be disabled if you don't like it via

```
config set modules.commandbox-update-check.enable=false
```

It will also automatically obey the [**offlineMode** Config Setting.](https://commandbox.ortusbooks.com/config-settings/misc-settings#offlinemode)

### Custom MIME Types

CommandBox will automatically set the content type in the HTTP response for common static file types. If you come across a file extension that doesn't have the correct type, you can set it like so in your `server.json`:

```
server set web.mimeTypes.log=text/plain
```

Which creates the following

```
{                            
    "web":{                             
        "mimeTypes":{                  
            "log":"text/plain"      
        }
    }
} 
```

In the above example, hitting a file such as `foo.log` would come back with a `text/plain` content type header.

This setting will override any `<mime-mapping>` tag in your `web.xml` file.

More Info: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/mime-types](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/mime-types)

### Config and Module Sync

If you are authenticated to ForgeBox in the CLI, you can synchronize your config settings to and from your ForgeBox account. This is a great way to get up and running on a new PC or keep multiple CommandBox installs in sync. In addition to synchronizing your Config Settings, this feature will also track your installed system modules, such as CFConfig, etc.

#### config sync push

This command will push your local settings and modules up to your ForgeBox account.

```
config sync push
```

By default, the settings are "merged" so new local settings will be added to ForgeBox, but nothing will be removed. To remove config that only exists on ForgeBox, you can use the `--overwrite` flag to force a full sync.

#### config sync pull

This command will pull your settings and modules from your ForgeBox account and set/install them locally.

```
config sync pull
```

By default, the settings are "merged" so missing settings will be added locally, and missing system modules will be installed, but nothing will be removed. To remove config and modules that only exist locally, you can use the `--overwrite` flag to force a full sync. This will remove local config settings and uninstall local system modules which were not on ForgeBox.

#### config sync diff

This command will not change anything, but gives you a full report of all settings which are different between your local CommandBox CLI and ForgeBox. It will show you "Remote Only," "Local Only," and "Changed" settings and modules. Use this to see what you're about to change before pushing or pulling.

```
config sync diff
```

Read More: [https://commandbox.ortusbooks.com/config-settings/setting-sync](https://commandbox.ortusbooks.com/config-settings/setting-sync)

### onServerInitialInstall interceptor

This is the same as `onServerInstall`, but it only runs the VERY FIRST time a CF engine is installed. This is helpful if you want to install Lucee extensions or ACF modules and only need to do it the first time. This interceptor is easier than using `onServerInstall` and inspecting the `installDetails.initialInstall` flag.

### Case Sensitivity of Web Server

This has been an experimental feature of CommandBox servers for a while, but we've finalized the feature and added a proper setting to enable it in server.json.  By default, the web server in CommandBox will follow the case sensitivity of the underlying file system. So, when on Windows `/FiLe.TxT` will still load an actual file called `/file.txt`. But on Linux, the case in the browser would need to match that of the file system. CommandBox allows you to force case sensitivity to be ON or OFF for a server, overriding the server's file system.

#### Forcing Case sensitivity

To force CommandBox's web server to be case sensitive, even on operating systems like Windows, use the following setting.  There is a nominal performance benefit in doing this, and it can allow a Windows CommandBox server to mimic a Linux server for testing.\
&#x20;

```
server set web.caseSensitivePaths=true
```

#### Forcing Case Insensitivity <a href="#forcing-case-insensitivity" id="forcing-case-insensitivity"></a>

To force CommandBox's web server to be case insensitive, even on operating systems like Linux, use the following setting. There is a nominal performance overhead in doing this, and it can allow a Linux CommandBox server to mimic a Windows IIS server. In this mode, CommandBox will use an internal cache of file system lookups to improve performance. If there are two files of the same name using different case, then you will get whatever file is found first.

```
server set web.caseSensitivePaths=false
```

Read More: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/case-sensitivity-of-web-server](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/case-sensitivity-of-web-server)&#x20;

### Support for PFX cert files  &#x20;

If using CommandBox's SSL, you can now use a PFX file (PKCS #8 format) which contains the public and private key in one file.

More Info: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/ssl-certs](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/ssl-certs)

### Case Insensitive Server Rule Predicates

Most of the Server Rule predicates are case-sensitive, which poses a problem when using them for security on Windows since they will only match one specific spelling of a folder or file.  We have added "-nocase" versions of several popular predicates which perform case-insensitive checks.

* regex-nocase()
* path-suffix-nocase()
* path-prefix-nocase()
* path-nocase()
* equals-nocase()
* contains-nocase()

### Server Rule Reverse Proxy handler supports SSL

Undertow's **reverse-proxy()** handler would not connect to a back-end server using SSL.  We've given up on [RedHat fixing this any time soon](https://issues.redhat.com/browse/UNDERTOW-1991), and added a new **load-balanced-proxy()** handler which works with SSL.

```
load-balanced-proxy({'https://reports1.mydomain.com','https://reports2.mydomain.com'})
```

### REPL Improvements

Due to [long-standing bugs](https://luceeserver.atlassian.net/browse/LDEV-1711) in the Lucee **evaluate()** function that seem like they'll [never get fixed](https://luceeserver.atlassian.net/browse/LDEV-1636), we've finally put a workaround in the REPL, which captures the return value of member functions chained to literals and expressions using closures. Ex:

```
CFSCRIPT-REPL: "test".len()
4
CFSCRIPT-REPL: [1,2,3].each( (i)=>echo(i) )
123
```

### Per-Server Preferred Browser Setting

There is already a Config Setting for the preferred browser when opening up sites.  You can now customize this on a per-server basis with this server.json setting

```
server set preferredBrowser=firefox
server open
```

### New Server Console Log Layouts

You can now control the Log4j appender layout for CommandBox servers, which includes formats such as JSON, which allows your server logs to be automatically imported into Elastic Search

```
server set runwar.console.appenderLayout=JSONTemplateLayout
```

Read More: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/console-log-layout](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/console-log-layout)

### New **forgebox version-debug** command

There is a helpful command called `forgebox version-debug` which will show you what version of a package will be installed without actually installing it. It can also be useful to test a semver range and see what packages it matches.

![](https://www.ortussolutions.com/\_\_media/forgebox-version-debug.png)

Read More: [https://commandbox.ortusbooks.com/package-management/installing-packages/debug-installation](https://commandbox.ortusbooks.com/package-management/installing-packages/debug-installation)

### Release Notes

#### Bug

[COMMANDBOX-1537](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1537) Experimental feature force insensitive web server has stopped working in some cases

[COMMANDBOX-1541](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1541) Hide Felix error messages in console on startup

[COMMANDBOX-1542](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1542) Custom tray options calling box with space in path fail

[COMMANDBOX-1550](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1550) Add load-balanced-proxy() handler to replace Undertow's broken reverse-proxy() because they refuse to fix it

[COMMANDBOX-1551](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1551) Capture return value from some REPL expressions because Lucee refuses to fix evaluate()'s parser

[COMMANDBOX-1552](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1552) Two instance of CLI cause class loading issues from OSGI bundles

[COMMANDBOX-1559](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1559) server start port check doesn't take web.http.enable into accout

#### New Feature

[COMMANDBOX-1434](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1434) CommandBox settings sync feature

[COMMANDBOX-1539](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1539) Add onServerInitialInstall package/server script

[COMMANDBOX-1540](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1540) Add \`.webp\` as a default mime type for CommandBox to support this new image format

[COMMANDBOX-1543](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1543) Formalize setting for case sensitivity of web server

[COMMANDBOX-1549](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1549) Add "nocase" versions of regex(), path-suffix(), path-prefix(), equals(), contains(), and path() predicates

[COMMANDBOX-1555](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1555) Improve forgebox whoami command

[COMMANDBOX-1556](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1556) Allow CommandBox to customize console appender Layout

[COMMANDBOX-1562](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1562) New "forgebox version-debug" command

[COMMANDBOX-1566](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1566) Bundle super helpful modules in box core

[COMMANDBOX-1567](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1567) onConfigSettingSave and onEndpointLogin interception announcements

#### Improvement

[COMMANDBOX-1034](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1034) Ability to pass file name to "more" command

[COMMANDBOX-1345](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1345) Add a method in server.json to add MIME type mappings to Undertow

[COMMANDBOX-1393](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1393) Improve message when starting second server with single server mode enabled

[COMMANDBOX-1538](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1538) system setting serverinfo namespace use interceptdata if running inside of server script

[COMMANDBOX-1544](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1544) Allow \`web.webroot\` to be changed in single server mode

[COMMANDBOX-1545](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1545) Authentication failures don't send custom error pages

[COMMANDBOX-1547](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1547) Add directory param to coldbox watch-reinit command

[COMMANDBOX-1548](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1548) Support PKCS #8 format private keys

[COMMANDBOX-1554](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1554) Allow preferredBrowser to be set on a per-server basis

[COMMANDBOX-1557](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1557) Add file and directory completion to the ID param of the install command

[COMMANDBOX-1558](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1558) Add installExtension() for commands and task runners to install Lucee extensions on the fly to the CLI

[COMMANDBOX-1560](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1560) Update Lucee to 5.3.10.120 in CLI core

[COMMANDBOX-1561](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1561) Improve upgrade command

[COMMANDBOX-1564](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1564) Load libdirs in system classloader

[COMMANDBOX-1565](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1565) Check for default branch of "main" in Git endpoint

#### Task

[COMMANDBOX-1357](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1357) Try removing JAX API classes from runwar

[COMMANDBOX-1546](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1546) Update to Undertow 2.2.22-Final

[COMMANDBOX-1553](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1553) Update bundled JRE to jdk-11.0.18+10

[COMMANDBOX-1563](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1563) Remove stopgap for COMMANDBOX-1459
