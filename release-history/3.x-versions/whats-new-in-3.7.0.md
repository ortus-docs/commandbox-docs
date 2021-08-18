# What's New in 3.7.0

## What's New

* **Task Runners** - Run ad-hoc builds from the CLI written in CFML [_\(Read more\)_](https://commandbox.ortusbooks.com/content/task-runners.html)
* **Manage System Packages** - update, list, and uninstall system modules [_\(Read more\)_](https://commandbox.ortusbooks.com/content/packages/installing_packages/system-modules.html)
* **File Globbing** - Use place holders like _\*\*.cfc_ for file operations to affect more than one file at a time. [_\(Read more\)_](https://commandbox.ortusbooks.com/content/usage/parameters/globbing-patterns.html)
* **Command Aliases** - Alias your favorite commands for easy access in the future [_\(Read more\)_](https://commandbox.ortusbooks.com/content/usage/execution/ad-hoc-command-aliases.html)
* **Global Command Parameter Defaults** - Set common parameters to have a given value at a global level [_\(Read more\)_](https://commandbox.ortusbooks.com/content/usage/execution/default-command-parameters.html)
* **System Settings** - Utilize environment variables to make your package and servers more dynamic [_\(Read more\)_](https://commandbox.ortusbooks.com/content/usage/execution/system-settings.html)
* **Testbox Run** - Improved, minimalist output to the "testbox run" command [_\(Read more\)_](https://commandbox.ortusbooks.com/content/testbox-integration/test-runner.html)
* **TestBox Watchers** - Watch a directory for file changes and run your unit tests [_\(Read more\)_](https://commandbox.ortusbooks.com/content/testbox-integration/test-watcher.html)
* **Customize REST Servlets** - Customize or disable the REST servlet paths on Lucee and Adobe servers [_\(Read more\)_](https://commandbox.ortusbooks.com/content/embedded_server/rest-servlet.html)
* **Custom Java Versions** - Start your CF servers with any version of Java you want [_\(Read more\)_](https://commandbox.ortusbooks.com/content/embedded_server/custom-java-version.html)
* **Property files** - New commands and helper libs for dealing with property files [_\(Read more\)_](https://commandbox.ortusbooks.com/content/task-runners/property-files.html)
* **Basic Authentication** - Enable basic security on your servers with unlimited users [_\(Read more\)_](https://commandbox.ortusbooks.com/content/embedded_server/basic-authentication.html)
* **Custom URL to Open** - Customize the browser URL that opens when you start a server [_\(Read more\)_](https://commandbox.ortusbooks.com/content/embedded_server/server_port_and_host.html#customize-url-that-opens-for-server)
* **Disable Tray Icon** - Turn off the system tray icon for your servers entirely [_\(Read more\)_](https://commandbox.ortusbooks.com/content/embedded_server/embedded_server.html#disable-the-tray-icon)
* **Show Proxy IP** - Servers pass through the original user IP through proxies
* **Jar Endpoint** - Install 3rd party jars into your projects [_\(Read more\)_](https://commandbox.ortusbooks.com/content/packages/endpoints/jar-via-http.html)

## Release Notes

### Bug

* \[[COMMANDBOX-176](https://ortussolutions.atlassian.net/browse/COMMANDBOX-176)\] - Server start tries to open HTTP URL even if it's disabled
* \[[COMMANDBOX-474](https://ortussolutions.atlassian.net/browse/COMMANDBOX-474)\] - testbox run with runner urls that have a query string fail
* \[[COMMANDBOX-525](https://ortussolutions.atlassian.net/browse/COMMANDBOX-525)\] - cf\_scripts folder not working on Adobe 2016
* \[[COMMANDBOX-600](https://ortussolutions.atlassian.net/browse/COMMANDBOX-600)\] - Catastrophic runner errors in testbox run don't fail tests
* \[[COMMANDBOX-611](https://ortussolutions.atlassian.net/browse/COMMANDBOX-611)\] - errors if you start second CLI while first one is using the temp dir
* \[[COMMANDBOX-616](https://ortussolutions.atlassian.net/browse/COMMANDBOX-616)\] - TestBox scaffolds are missing super calls for beforeAll/afterAll
* \[[COMMANDBOX-621](https://ortussolutions.atlassian.net/browse/COMMANDBOX-621)\] - Prevent two servers from getting the same name
* \[[COMMANDBOX-625](https://ortussolutions.atlassian.net/browse/COMMANDBOX-625)\] - Basic auth doesn't set cgi.remote\_user
* \[[COMMANDBOX-651](https://ortussolutions.atlassian.net/browse/COMMANDBOX-651)\] - unregister method in interceptor service doesn't work

### New Feature

* \[[COMMANDBOX-15](https://ortussolutions.atlassian.net/browse/COMMANDBOX-15)\] - Allow file globbing patterns in file/folder operations
* \[[COMMANDBOX-50](https://ortussolutions.atlassian.net/browse/COMMANDBOX-50)\] - Create BaseTask
* \[[COMMANDBOX-51](https://ortussolutions.atlassian.net/browse/COMMANDBOX-51)\] - Add "task" command to run tasks
* \[[COMMANDBOX-54](https://ortussolutions.atlassian.net/browse/COMMANDBOX-54)\] - Create watchers
* \[[COMMANDBOX-459](https://ortussolutions.atlassian.net/browse/COMMANDBOX-459)\] - Create the --system argument to all package commands for system wide packages
* \[[COMMANDBOX-513](https://ortussolutions.atlassian.net/browse/COMMANDBOX-513)\] - Allow REST servlet to be configured
* \[[COMMANDBOX-548](https://ortussolutions.atlassian.net/browse/COMMANDBOX-548)\] - Allow custom JRE version for server starts
* \[[COMMANDBOX-560](https://ortussolutions.atlassian.net/browse/COMMANDBOX-560)\] - Support Basic Auth
* \[[COMMANDBOX-564](https://ortussolutions.atlassian.net/browse/COMMANDBOX-564)\] - Allow placeholders in for env vars and system props
* \[[COMMANDBOX-585](https://ortussolutions.atlassian.net/browse/COMMANDBOX-585)\] - Provide convenient command to do simple token replacements from the CLI
* \[[COMMANDBOX-589](https://ortussolutions.atlassian.net/browse/COMMANDBOX-589)\] - Checksum Command
* \[[COMMANDBOX-590](https://ortussolutions.atlassian.net/browse/COMMANDBOX-590)\] - Property files commands support
* \[[COMMANDBOX-599](https://ortussolutions.atlassian.net/browse/COMMANDBOX-599)\] - Add MinHeapSize setting
* \[[COMMANDBOX-608](https://ortussolutions.atlassian.net/browse/COMMANDBOX-608)\] - Support for viewing/installing private packages
* \[[COMMANDBOX-610](https://ortussolutions.atlassian.net/browse/COMMANDBOX-610)\] - Finalize box.json testbox runner options
* \[[COMMANDBOX-613](https://ortussolutions.atlassian.net/browse/COMMANDBOX-613)\] - Allow Command DSL to set working directory
* \[[COMMANDBOX-614](https://ortussolutions.atlassian.net/browse/COMMANDBOX-614)\] - Implement "testbox watch" command
* \[[COMMANDBOX-638](https://ortussolutions.atlassian.net/browse/COMMANDBOX-638)\] - Simple Jar endpoint
* \[[COMMANDBOX-642](https://ortussolutions.atlassian.net/browse/COMMANDBOX-642)\] - Ability to disable tray icons
* \[[COMMANDBOX-644](https://ortussolutions.atlassian.net/browse/COMMANDBOX-644)\] - Allow ad-hoc aliases to be created for commands
* \[[COMMANDBOX-645](https://ortussolutions.atlassian.net/browse/COMMANDBOX-645)\] - Allow global defaults to be set for command parameters
* \[[COMMANDBOX-647](https://ortussolutions.atlassian.net/browse/COMMANDBOX-647)\] - Automatic collection from parameter names containing a colon
* \[[COMMANDBOX-648](https://ortussolutions.atlassian.net/browse/COMMANDBOX-648)\] - Implement the equiv of Tomcat's remoteIPValve
* \[[COMMANDBOX-649](https://ortussolutions.atlassian.net/browse/COMMANDBOX-649)\] - Support missing Tuckey config settings
* \[[COMMANDBOX-655](https://ortussolutions.atlassian.net/browse/COMMANDBOX-655)\] - Command to remove trailing whitespace from files
* \[[COMMANDBOX-656](https://ortussolutions.atlassian.net/browse/COMMANDBOX-656)\] - Command to add final EOL to files

### Task

* \[[COMMANDBOX-568](https://ortussolutions.atlassian.net/browse/COMMANDBOX-568)\] - Better error message for invalid JSON in a server.json file

### Improvement

* \[[COMMANDBOX-429](https://ortussolutions.atlassian.net/browse/COMMANDBOX-429)\] - Update debian build signing to be higher than SHA-256
* \[[COMMANDBOX-586](https://ortussolutions.atlassian.net/browse/COMMANDBOX-586)\] - If publishing but not logged into forgebox, prompt for login instead of just erroring
* \[[COMMANDBOX-597](https://ortussolutions.atlassian.net/browse/COMMANDBOX-597)\] - Clean up SSL cert and key file parameters for server start
* \[[COMMANDBOX-601](https://ortussolutions.atlassian.net/browse/COMMANDBOX-601)\] - Improve HTML to ANSI conversion on larger strings
* \[[COMMANDBOX-602](https://ortussolutions.atlassian.net/browse/COMMANDBOX-602)\] - Refactor JSON formatter to separate lib for reuse
* \[[COMMANDBOX-606](https://ortussolutions.atlassian.net/browse/COMMANDBOX-606)\] - Add trace flag for starting server
* \[[COMMANDBOX-612](https://ortussolutions.atlassian.net/browse/COMMANDBOX-612)\] - Improve output of "testbox run" command
* \[[COMMANDBOX-617](https://ortussolutions.atlassian.net/browse/COMMANDBOX-617)\] - Remove deprecated and unused properties from box.json with init
* \[[COMMANDBOX-618](https://ortussolutions.atlassian.net/browse/COMMANDBOX-618)\] - Don't try to output binary data in REPL
* \[[COMMANDBOX-622](https://ortussolutions.atlassian.net/browse/COMMANDBOX-622)\] - Show "last started" datetime for servers
* \[[COMMANDBOX-623](https://ortussolutions.atlassian.net/browse/COMMANDBOX-623)\] - Customize URL that opens when starting server
* \[[COMMANDBOX-626](https://ortussolutions.atlassian.net/browse/COMMANDBOX-626)\] - Allow commandbox-modules to register endpoints
* \[[COMMANDBOX-646](https://ortussolutions.atlassian.net/browse/COMMANDBOX-646)\] - Enhance parser to allow quoted spaces in parameter names
* \[[COMMANDBOX-650](https://ortussolutions.atlassian.net/browse/COMMANDBOX-650)\] - WireBox injection DSL allow to drill down into Config Settings
* \[[COMMANDBOX-652](https://ortussolutions.atlassian.net/browse/COMMANDBOX-652)\] - Command to remove trailing spaces from code files
* \[[COMMANDBOX-657](https://ortussolutions.atlassian.net/browse/COMMANDBOX-657)\] - Allow console flag to be stored in server.json like every other setting
* \[[COMMANDBOX-658](https://ortussolutions.atlassian.net/browse/COMMANDBOX-658)\] - Allow publishing of private packages

