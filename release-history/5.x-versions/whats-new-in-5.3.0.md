# What's New in 5.3.0

### Override Config Settings via Env Vars

Every Config Setting can be overridden by convention by creating environment variables in the shell where you run `box`. This is ideal for CI builds where you want to easily set ForgeBox API keys, or tweak settings for your build. 

```bash
box_config_endpoints_forgebox_APIToken=my-token-here

# JSON which will be parsed
box_config_proxy={ "server" : "localhost", "port": 80 }

# dot-delimited keys (windows only)
box_config_endpoints.forgebox.APIToken=my-token-here

# array indexes too (windows only)
box_config_foo.bar[baz].bum[1]=test
```

More Info: [https://commandbox.ortusbooks.com/config-settings/env-var-overrides](https://commandbox.ortusbooks.com/config-settings/env-var-overrides)

### Override Server Settings via Env Vars

Every server setting can be overridden by convention by creating environment variables in the shell where you run `box`. This is ideal for CI builds where you want to easily set ports, or tweak settings for your build.

```bash
box_server_profile=production

box_server_web_http_port=8080

# JSON which will be parsed
box_server_web_ssl={ "enabled" : true, "port": 443 }

# dot-delimited keys (Windows only)
box_server_web.http.port=8080

# array indexes too (Windows only)
box_server_web_rules[1]=path-suffix(/box.json) -> set-error(404)
box_server_web_rules[2]=disallowed-methods(trace)
```

More Info: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/env-var-overrides](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/env-var-overrides)

### HTTP/2 Support

CommandBox now has out-of-the-box support for the HTTP/2 protocol.  It is always enabled by default and browsers will use it when you're serving over HTTPS.

```bash
server set web.http2.enable=true/false
```

More Info: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/server-port-and-host\#http-2](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/server-port-and-host#http-2)

### JMES JSON filtering / jq Command

Thanks to a massive effort from Scott Steinbeck, the CFML world has a new [CF implementation](https://www.forgebox.io/view/jmespath) of the [JMES spec](https://jmespath.org/), which is what powers the popular ["jq" \(or JSON Query\) bash command](https://stedolan.github.io/jq/). We've plugged this new library into CommandBox and exposed it in the following ways.

We've added a new **jq** command which behaves roughly like the bash counterpart.  You can pipe in JSON, or read the JSON from a file and apply a JSON query against it which can be used to filter, massage, rewrite, map, or filter the JSON into a new JSON object.

```bash
# Return array of dependency names
package show | jq keys(dependencies)

# Find dependencies with "cb" in their name
package show | jq key_contains(dependencies,'cb')

```

We've also added the ability to specify powerful **jq** filters to the "**package show**", "**server show**", and "**config show**" commands directly.  Just prefix your filter with the text "jq:" like so:

```bash
config show jq:endpoints.forgebox.apiToken
# .. is the same as ...
config show endpoints.forgebox.apiToken

# or you can get fancy...
config show 'jq:keys(modules)'

# Impress your friends
package show "jq:[name,version]"

# Be the life of the party
package show "jq:contributors|split(@,' ')" 
```

The **jq** command and **JMES** spec are very powerful and probably do [much more than you realize](https://jmespath.org/tutorial.html)!  Make sure you check out the docs for more ideas.

More Info: [https://commandbox.ortusbooks.com/usage/jq-command](https://commandbox.ortusbooks.com/usage/jq-command)

### AJP Secret Support

CommandBox's AJP listener \(provided by Undertow\) is already protected against the [Ghostcat vulnerability](https://www.synopsys.com/blogs/software-security/ghostcat-vulnerability-cve-2020-1938/). However, if you would like to set up an AJP secret as well to ensure all requests coming into the AJP listener are from a trusted source, you can do this by setting the `web.ajp.secret` property.

```bash
server set web.AJP.secret=mySecret
```

For this to work, you must also configure your AJP proxy in your web server to send the same secret!

More info: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/server-port-and-host\#ajp-secret](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/server-port-and-host#ajp-secret)

### AsyncManager Available to Task Runners and Commands

We've updated the version of WireBox inside the CLI and now have access to the AsyncManager for sweet threading and scheduled task support. 

```javascript
// Parallel Executions
async().all(
    () => hyper.post( "/somewhere" ),
    () => hyper.post( "/somewhereElse" ),
    () => hyper.post( "/another" )
).then( (results)=> logResults( results ) );
```

CommandBox is using an AsyncManager scheduled task thread now to redraw interactive jobs and progress bars.  Look out for some new eye candy hiding in your server starts and package installs!

More Info: [https://commandbox.ortusbooks.com/task-runners/threading-async\#asyncmanager](https://commandbox.ortusbooks.com/task-runners/threading-async#asyncmanager)

### New Table Printer 

The print helper in commands and Task Runners has a new toy that will print ASCII representations of tabular data thanks to a pull request from Eric Peterson.  You can see it in the output of the **outdated** command. 

![](https://www.ortussolutions.com/__media/blog/commandbox-outdated-screenshot.png)

And you can use it in your Task Runners like so:

```javascript
print.table(
	[ 'First Name', 'Last Name' ],
	[
		[ 'Brad', 'Wood' ],
		[ 'Luis', 'Majano' ],
		[ 'Gavin', 'Pickin' ]
	]
);
```

### ColdBox Scaffolding for REST Handlers

When scaffolding ColdBox handlers, we have support for ColdBox 6.x REST Handlers now.

```bash
coldbox create handler --rest
```

### Experimental Server Features

You can enable extra Resource Manager Logging when troubleshooting file system issues:

```bash
server set runwar.args="--resource-manager-logging=true"
```

You can force case sensitivity on a Windows server:

```bash
server set runwar.args="--case-sensitive-web-server=true"
```

You can force case Insensitivity on a Linux server:

```bash
server set runwar.args="--case-sensitive-web-server=false"
```

You can enable a cache of file system lookups of servlet paths.  This is only for production and will eliminate repeated file system hits by your CF engine, such as checking for an Application.cfc file on every request, or testing where the servlet context root is.  Standard Adobe ColdFusion installations have a similar cache of "real" paths from the servlet context that is tied to a setting in the administrator called "Cache Webserver paths" but that setting is not available and does not work on CommandBox servers for some reason.  This setting would apply to any CF engine.

```bash
server set runwar.args="--cache-servlet-paths=true"
```

More Info: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/experimental-features](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/experimental-features)

### HTTPS Redirect/HSTS

When using a CommandBox web server in production, you may wish to force your users to visit your site over HTTPS for security \(and for HTTP/2 to work\). However, it is desirable to still have your web server listening on HTTP so a user just typing your address in his browser can still connect to HTTP and then redirect.  CommandBox can be configured to redirect all HTTP traffic over to HTTPS with the following setting.

```bash
server set web.SSL.forceSSLRedirect=true
```

If you want to go one step further, you can add a `Strict-Transport-Security` header to your site. This instructs the browser to _automatically_ use HTTPS every time the user visits your site again.

```bash
server set web.SSL.HSTS.enable=true
server set web.SSL.HSTS.maxAge=31536000
server set web.SSL.HSTS.includeSubDomains=true
```

More Info: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/https-redirect-hsts](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/https-redirect-hsts)

### Force Colored Output in your Builds

CommandBox won't use ANSI color formatting when running inside of a non-interactive terminal.  However, build servers such as Gitlab or Jenkins \(via a plugin\) support ANSI color sequences.  You can force CommandBox to use colored text output with this new setting:

```bash
config set colorInDumbTerminal=true
```

### Loose Semantic Version Parsing

One of the common hangups for people dealing with Lucee Server and Adobe ColdFusion CF Engines versions, is that CommandBox follows the npm-flavor of the semantic version spec and expects

```bash
server start cfengine=lucee@5.3.7+48
```

instead of 

```bash
server start cfengine=lucee@5.3.7.48
```

So we've loosened our sem ver library to treat the 4th number as a build ID if there is no plus sign in the version \(instead of just discarding the 4th digit as the spec requires\)

### Support for "localhost subdomains"

Most modern browsers allow you to make up any subdomain you want before localhost such as **mySite.localhost** and will simply resolve them to localhost \(127.0.0.1\) even without a hosts file entry.  CommandBox now supports using these domains and will bind your server's ports to localhost even without using the **commandbox-hostupdater** module.

```bash
server set web.host=mySite.localhost
```

More Info: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/server-port-and-host\#a-gracious-host](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/server-port-and-host#a-gracious-host)

### Relative CommandBox home

You can customize where CommandBox lives by placing a **commandbox.properties** file next to the box binary.  We have better support for relative paths now so you can have portable CommandBox installations such as a thumb drive.

```bash
commandbox_home=../customHome
```

More Info: [https://commandbox.ortusbooks.com/setup/installation](https://commandbox.ortusbooks.com/setup/installation)

### Halt Server If Port In Use \(Breaking Change\)

The only known breaking change in this release is if you try to start two servers on the same HTTP port.  Previously, CommandBox would just ignore the port on the second server and choose a random port.  Due to the confusion that can cause, CommandBox will now throw an error.  If you want to override an explicit port locally, set the port to an empty string or a 0 and CommandBox will choose a random port for you.  For example, if you are using the **commandbox-dotenv** module, you can put this line in your project's **.env** file to override the port in your **server.json**

```bash
box_server_web_http_port=0
```

### Relative Web Alias Behavior \(regression\)

If you have a server with the **server.json** outside of the web root and at least one relative web alias, the alias will not work on the first start of the server.  The workaround is to change the web aliases to be relative to the folder that the **server.json** lives in.  

### Incompatibility with old DotEnv module

Some users receive the following error when starting CommandBox after updating:

```text
The parameter [name] to function [get] is required but was not passed in.
```

If you see this, it means you have an older version of the **commandbox-dotenv** module installed that is not compatible with the new version of WireBox inside CommandBox.  To fix, delete this folder out of your CommandBox home:

```text
~/.CommandBox/cfml/modules/commandbox-dotenv
```

Now the CLI will start and you can install the latest version of dotenv.

```text
install commandbox-dotenv
```

## Release notes

Here is the list of all tickets included in the 5.3.0 release.

### Bug

[COMMANDBOX-1301](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1301) web server aliases in server.json should be relative to the folder of the server.json

[COMMANDBOX-1300](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1300) ${Setting: serverinfo.foo not found} expansions don't work in a folder that's not the web root

[COMMANDBOX-1291](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1291) Re-using same server.json with two names doesn't work

[COMMANDBOX-1276](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1276) Corrupted WireBox metadata cache file will prevent CommandBox from starting

[COMMANDBOX-1275](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1275) HTTP2 Additional Port Handling and Flexibility

[COMMANDBOX-1271](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1271) REPL & Command highlighters don't handle square brackets \[\] well

[COMMANDBOX-1270](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1270) JVM arg ending in backslash doesn't work

[COMMANDBOX-1268](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1268) Coldbox Watch-Reinit Watches Unwanted Folders

[COMMANDBOX-1263](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1263) Package installation doesn't always optimize duplicate packages

[COMMANDBOX-1261](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1261) Globber.count\(\) bombs if run after .asQuery\(\)

[COMMANDBOX-1259](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1259) Starting lucee@1.2.3 will use light-light when using CommandBox Light

[COMMANDBOX-1255](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1255) Loading class files in task runner doesn't work

[COMMANDBOX-1253](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1253) variables scope doesn't persist between task dependencies

[COMMANDBOX-1250](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1250) tokenreplace removes BOM from files

[COMMANDBOX-1212](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1212) trayOptions.json not respecting serverHomeDirectory

[COMMANDBOX-664](https://ortussolutions.atlassian.net/browse/COMMANDBOX-664) Server status not always correct.

### Improvement

[COMMANDBOX-1297](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1297) Add singleServerHome option to not auto-deploy different versions of servers

[COMMANDBOX-1296](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1296) Improve error message if version isn't found in ForgeBox

[COMMANDBOX-1295](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1295) Loosen parsing of build ID in semver

[COMMANDBOX-1292](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1292) Treat empty HTTP port the same as 0 \(chooses random port\)

[COMMANDBOX-1290](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1290) Remove runwar hack that sets java vesrion

[COMMANDBOX-1288](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1288) Runwar timesout when Lucee's new warmup flag is used

[COMMANDBOX-1287](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1287) Add option for PID file

[COMMANDBOX-1285](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1285) Allow generic override of server start settings via env vars or java sys props

[COMMANDBOX-1284](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1284) Have parser ignore quotes inside a token

[COMMANDBOX-1269](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1269) Cache "/" path lookup in Runwar's mapped resource manager

[COMMANDBOX-1267](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1267) Add setters for run\(\) arguments in CommandDSL

[COMMANDBOX-1258](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1258) Default embedded server only needs to copy lucee.jar

[COMMANDBOX-1256](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1256) Have `outdated` also show latest version of a package

[COMMANDBOX-1252](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1252) Integrate AsyncManager into CommandBox

[COMMANDBOX-1251](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1251) Upgrade to latest WireBox 6.x

[COMMANDBOX-1249](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1249) Bundle testbox in testbox module to prevent auto download

[COMMANDBOX-1248](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1248) Halt server start if asked for port is in use

[COMMANDBOX-1246](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1246) the git bullet train car disappears while current working directory is not the root project folder

[COMMANDBOX-1216](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1216) Support AJP secret in Undertow

[COMMANDBOX-1169](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1169) Rethink the way the screen is redrawn upon extensive installs so it can be fluent on all screen sizes

[COMMANDBOX-1136](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1136) Can't link package if no modules are installed

[COMMANDBOX-1117](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1117) New first-class setting to enable HTTP2

[COMMANDBOX-1108](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1108) Comment out the default environment vars in .env when createing a new coldbox app

### New Feature

[COMMANDBOX-1294](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1294) Allow server rules to be commented out with \#

[COMMANDBOX-1293](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1293) Allow servers to use random.localhost domains

[COMMANDBOX-1289](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1289) Integrate JMES JSON filtering

[COMMANDBOX-1282](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1282) Debug when lucee-extensions don't find Lucee server

[COMMANDBOX-1281](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1281) Allow JSON service to create implicit arrays

[COMMANDBOX-1280](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1280) Add config setting to enable ANSI colors in dumb terminals

[COMMANDBOX-1279](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1279) Allow generic override of config settings via env vars or java sys props

[COMMANDBOX-1278](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1278) Add --json flag to server list

[COMMANDBOX-1277](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1277) Add HTTP redirect options

[COMMANDBOX-1273](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1273) Add optional servlet path cache in Runwar

[COMMANDBOX-1262](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1262) Allow caching of task runners

[COMMANDBOX-1260](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1260) Support for generating ColdBox RESTHandlers

[COMMANDBOX-1245](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1245) Support default module export as @moduleName,

[COMMANDBOX-676](https://ortussolutions.atlassian.net/browse/COMMANDBOX-676) Allow commandbox\_home to be relative

