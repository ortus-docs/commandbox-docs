# What's New in 5.4.0

### web.xml Overrides

When you start an Adobe or Lucee CF Engine, the WAR CommandBox uses has a stock **web.xml** baked into it.  Sometimes you may want to add custom servlets, servlet mappings, etc into your server.  You can override the stock **web.xml** in part or in total with a file of your own which you specify in the **server.json** like so:

```
{
  "app" : {
    "webXMLOverride" : "path/to/web-override.xml"
  }
}
```

More info here:&#x20;

[https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/web.xml-overrides](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/web.xml-overrides)

### Funky Parameters

\
In addition to quoting parameter values, parameter names can also be quoted.  This is useful when setting keys into settings or JSON files that have spaces, hyphens or special characters.  Each of these examples are now supported:

```
# quoted string
package set foo."bar.baz"=bum

# bracketed string
package set foo[bar.baz]=bum

# quoted, bracketed string
package set foo["bar.baz"]=bum
```

More info here:

[https://commandbox.ortusbooks.com/usage/parameters](https://commandbox.ortusbooks.com/usage/parameters)

### Library updates

**Lucee Server** has been updated to **5.3.8.201** and **JBoss Undertow** has been updated to **2.2.10.Final**.  The Lucee update, as usual, applies to the CLI as well as the default server you get when you run **server start**.

### Smarter Jar Endpoint

When you install a Jar via HTTP URL and the version number is baked into the URL, the jar endpoint now makes more assumptions about what the version of the package is that allows it to optimize the update checks and eliminate unnecessary downloads.&#x20;

More info here:

[https://commandbox.ortusbooks.com/package-management/code-endpoints/jar-via-http#semantic-versioning](https://commandbox.ortusbooks.com/package-management/code-endpoints/jar-via-http#semantic-versioning)

### ask and confirm commands

Here are some fun commands for user interactivity in the shell.  You can use these as part of a recipe or a nice "one-liner".

#### ask

The ask command is similar to the **ask()** method in Task Runners.  It requires an interactive terminal and will ask the user a question and return their answer.  It is meant to be changed with other commands.

```
set color=`ask "favorite Color? "`
echo "you said ${Setting: color not found}"
```

or with default values

```
ask question="Who is cool? " defaultResponse="Balbino!"
```

or with masked input

```
ask question="What is your password? " mask=*
```

Or fun stuff like this

```
ask "Secret phrase: " | assertEqual "mockingbird" || echo "access denied!" && exit 1
```

#### confirm

The confirm command will ask the user a yes/no question and return a passing or failing exit code from the command based on the answer.

```
confirm "do you want to update your packages? " && update
```

Remember the && operator will only execute the second command if the first command returns an exit code of 0.

More info here:

[https://commandbox.ortusbooks.com/helpful-commands/ask-and-confirm](https://commandbox.ortusbooks.com/helpful-commands/ask-and-confirm)

### server prune command

You can easily forget all servers which have not been started for a certain period of time with the server prune command.  It accepts the number of days that need to have passed since a server was last started in order to prune it.

```
server prune days=30

# Skip the confirmation check
server prune --force
```

More info here:&#x20;

[https://commandbox.ortusbooks.com/embedded-server/manage-servers#prune-old-servers](https://commandbox.ortusbooks.com/embedded-server/manage-servers#prune-old-servers)

### Release Notes

Here are the full release notes for CommandBox 5.4.0

### Bug

[COMMANDBOX-1364](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1364) Cancelling a prompt with active job doesn't clear job logs

[COMMANDBOX-1361](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1361) Param tab completion is off-by-one when piping

[COMMANDBOX-1360](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1360) Can't list files in directory with brackets ( \[ or ] ) in the name

[COMMANDBOX-1356](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1356) forgebox timeout is too small when publishing packages

[COMMANDBOX-1354](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1354) dir command returns no results in drive root

[COMMANDBOX-1353](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1353) Summary over 200 chars in box.json causes error when publishing

[COMMANDBOX-1352](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1352) Addition of Apache logging classes breaks 3rd party libs using it

[COMMANDBOX-1350](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1350) Installing a system module as one-off command doesn't clear wirebox metadata cache

[COMMANDBOX-1348](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1348) Commenting server rules doesn't work correctly in text files

[COMMANDBOX-1346](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1346) CommandDSL doesn't handle struct args

[COMMANDBOX-1344](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1344) create a server prune command

[COMMANDBOX-1339](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1339) Server start can hang when CF engine blows up

[COMMANDBOX-1338](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1338) Tab complete doesn't work after a pipe

[COMMANDBOX-1337](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1337) Working dir of server custom menu items doesn't default properly

[COMMANDBOX-1336](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1336) Error when setting failing exist code in Task Runner

[COMMANDBOX-1334](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1334) updating commandbox to 5.3.1 via Homebrew breaks with a java error

[COMMANDBOX-1333](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1333) Rewrite rule with query string doubles up question mark

[COMMANDBOX-1332](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1332) printTable column validation breaks with spaces in list

[COMMANDBOX-1330](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1330) Directory listing not showing folders properly when names are numeric

[COMMANDBOX-1230](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1230) Certain Java installs fail version check

### Improvement

[COMMANDBOX-1366](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1366) ask and confirm command to capture user input from shell

[COMMANDBOX-1365](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1365) Improve version handling in JAR endpoint

[COMMANDBOX-1351](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1351) Update to Lucee 5.3.8

[COMMANDBOX-1347](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1347) Support dots in struct keys with set/show/clear commands

[COMMANDBOX-1342](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1342) printTable custom header names for non-array input

[COMMANDBOX-1331](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1331) Add printTable check for data with no columns

[COMMANDBOX-1329](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1329) Sort column names in printTable --debug

### New Feature

[COMMANDBOX-1362](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1362) Set env vars directly in server.json for local one-off overrides

[COMMANDBOX-1011](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1011) Support web.xml Overrides

### Task

[COMMANDBOX-1363](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1363) Update to Undertow 2.2.10.Final
