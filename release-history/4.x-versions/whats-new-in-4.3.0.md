# What's New in 4.3.0

### Task Target Dependencies

For a task that has more than one target \(method\) you can specify dependencies that will run, in the order specified, prior to your final target. Specify task target dependencies as a comma-delimited list in a `depends` annotation on the target function itself. There is no limit to how many target dependencies you can have, nor how deep they can nest.

```javascript
component {
  
  function run() depends="runMeFirst" {
  }

  function runMeFirst() {
  }

}
```

Given the above Task Runner, typing

```bash
task run
```

would run the **`runMeFirst()`** and **`run()`** method in that order.  
  

Docs:

[https://commandbox.ortusbooks.com/task-runners/task-target-dependencies](https://commandbox.ortusbooks.com/task-runners/task-target-dependencies)

### GZip Compression

The web server in CommandBox is capable of enabling GZIp compression to reduce the size of HTTP responses. To enable GZip compress on your CommandBox server, add a **`web.gzipEnable`** setting in your `server.json` file.

```bash
server set web.gzipEnable=true
```

Docs: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/gzip-compression](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/gzip-compression)

### Add --simple flag to ls/dir command

When you get a directory listing in CommandBox, you can add the **--simple** flag which will only output the file and folder name without any other information.  This feature was added to compliment the feature below.

```bash
ls --simple
```

### forEach Command

The **`foreach`** command will execute another command against every item in an incoming list. The list can be passed directly or piped into this command. The default delimiter is a new line so this works great piping the output of file listings directly in, which have a file name per line.

This example will find all JSON files in a directory and run the **cat** command against them.

```bash
ls *.json --simple | forEach cat
```

Docs: [https://commandbox.ortusbooks.com/usage/foreach-command](https://commandbox.ortusbooks.com/usage/foreach-command)

### Java 9/10/11 support

This is still a little experimental since it hasn't gone through full testing, but we upgraded to Lucee 5.2.9.31 in the core CLI which has support for the newer versions of Java.  We've removed the checks that previously preventing CommandBox from even trying to run on versions of Java later than 8 and at first glance it seems to be working though there's been some flakiness on Java 11.  Please help test these later Java versions and remember that if you spin up a server, you'll want to still dial in Java 8 for Adobe CF 2016 and prior and Lucee 5.2.8.50 and prior even if you have the CLI running on Java 9+.

Docs for setting custom Java version in your server: 

[https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/custom-java-version](http://server: https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/custom-java-version)

## Release notes

Here's the full release notes for CommandBox 4.3.0.

### Bug

* \[[COMMANDBOX-692](https://ortussolutions.atlassian.net/browse/COMMANDBOX-692)\] - Command Box failed to initialize using java 9
* \[[COMMANDBOX-845](https://ortussolutions.atlassian.net/browse/COMMANDBOX-845)\] - CFFileServlet doesn't work with default rewrites in ACF 2016
* \[[COMMANDBOX-849](https://ortussolutions.atlassian.net/browse/COMMANDBOX-849)\] - We need to review exit codes in Tasks
* \[[COMMANDBOX-856](https://ortussolutions.atlassian.net/browse/COMMANDBOX-856)\] - WireBox/LogBox upgrade broke system logging
* \[[COMMANDBOX-857](https://ortussolutions.atlassian.net/browse/COMMANDBOX-857)\] - Engine name not detected correctly when using HTTP URL for cfengine
* \[[COMMANDBOX-860](https://ortussolutions.atlassian.net/browse/COMMANDBOX-860)\] - Fix annoying web-inf folder for Flex logs on Adobe engines
* \[[COMMANDBOX-861](https://ortussolutions.atlassian.net/browse/COMMANDBOX-861)\] - Missing line break when following log file
* \[[COMMANDBOX-865](https://ortussolutions.atlassian.net/browse/COMMANDBOX-865)\] - Spelling error in info message for accessLogEnable
* \[[COMMANDBOX-867](https://ortussolutions.atlassian.net/browse/COMMANDBOX-867)\] - coldbox create app cuts last char from package name
* \[[COMMANDBOX-869](https://ortussolutions.atlassian.net/browse/COMMANDBOX-869)\] - Starting Adobe server errors when no CFIDE mapping is defined
* \[[COMMANDBOX-871](https://ortussolutions.atlassian.net/browse/COMMANDBOX-871)\] - CommandDSL parsing doesn't handle quoted text in command

### New Feature

* \[[COMMANDBOX-848](https://ortussolutions.atlassian.net/browse/COMMANDBOX-848)\] - Task method dependencies
* \[[COMMANDBOX-852](https://ortussolutions.atlassian.net/browse/COMMANDBOX-852)\] - Add setting for GZip compression
* \[[COMMANDBOX-858](https://ortussolutions.atlassian.net/browse/COMMANDBOX-858)\] - Add --simple flag to ls/dir command to only output filename
* \[[COMMANDBOX-859](https://ortussolutions.atlassian.net/browse/COMMANDBOX-859)\] - Add "forEach" command to execute command once per incoming line

### Improvement

* \[[COMMANDBOX-824](https://ortussolutions.atlassian.net/browse/COMMANDBOX-824)\] - TestBox Run command could use a way to add custom url parameters. Also the options parameter does nothing
* \[[COMMANDBOX-846](https://ortussolutions.atlassian.net/browse/COMMANDBOX-846)\] - Improve progress bar cleanup and exit codes on Ctrl-C
* \[[COMMANDBOX-851](https://ortussolutions.atlassian.net/browse/COMMANDBOX-851)\] - Allow Java jars to be installed from S3
* \[[COMMANDBOX-854](https://ortussolutions.atlassian.net/browse/COMMANDBOX-854)\] - JSON Schema for server.json
* \[[COMMANDBOX-863](https://ortussolutions.atlassian.net/browse/COMMANDBOX-863)\] - Upgrade CLI to Lucee 5.2.9.31
* \[[COMMANDBOX-864](https://ortussolutions.atlassian.net/browse/COMMANDBOX-864)\] - Support sorting JSON objects by key when formatting
* \[[COMMANDBOX-866](https://ortussolutions.atlassian.net/browse/COMMANDBOX-866)\] - Task DSL assume CWD of task file
* \[[COMMANDBOX-868](https://ortussolutions.atlassian.net/browse/COMMANDBOX-868)\] - coldbox scaffold install testbox by default
* \[[COMMANDBOX-870](https://ortussolutions.atlassian.net/browse/COMMANDBOX-870)\] - Allow command DSL params\(\) to be called more than once
* \[[COMMANDBOX-872](https://ortussolutions.atlassian.net/browse/COMMANDBOX-872)\] - Make resolvePath\(\) in Base command/task
* \[[COMMANDBOX-873](https://ortussolutions.atlassian.net/browse/COMMANDBOX-873)\] - Reload shell doesn't always fire when non-CommandBox modules get updated in core
* \[[COMMANDBOX-874](https://ortussolutions.atlassian.net/browse/COMMANDBOX-874)\] - Allow print helper to accept complex objects and serialize them for output.

