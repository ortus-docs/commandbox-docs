# What's New in 4.0.0

## Major Areas of Development

* Major rewrite of CLI engine loader
  * **Lucee 5** now powers the CLI
  * Using **JSR-223** to dynamically load Lucee 5
* All 3rd Party libs updated
  * JGit
  * Launch4J
  * Runwar
  * JLine3
* Improved Task Runner support
  * Task scaffolding with “**task create**”
  * Task DSL to call other tasks
  * Ortus Builds are now being converted to Task Runners.  No Ant!  No XML!
* Support for Private package
* Revamped Server Logs \(access, rewrite, console\)
* ColdBox 5 updates
* Tons of bug fixes and improvements

## Featured Enhancements

There are currently 69 tickets that are part of the 4.0 release.  You can view them all over [in JIRA](https://ortussolutions.atlassian.net/browse/COMMANDBOX) if you filter based on a **fixVersion** of **4.0.0**.  Here's an overview of some of the cooler new features, in no particular order.

### 256 Color support

This is dependant on your terminal, but the CLI now can look a lot prettier. For Mac users, this will probably work out of the box.  For Windows users, **cmd** won't cut it.  We recommend checking out [**ConEMU**](https://conemu.github.io/) as a sweet, tabbed terminal replacement.   To see what kind of punch your terminal packs, run this new command:

```text
system-colors
```

![](https://lh6.googleusercontent.com/FdxSovIc7TYpRMbNsOryEFKfE0y_ArEUXZD__ISZvLwXy8LRb0Ws6KFPEljDYcTrLHPvunvb2LSH40bTlSMjeYOXY0bU4-7U77uSagBrU9rLWH5CbXGvoQNB6qbe9QjCBgEC94a9VaQ)

### CLI Improvements

Along with the newest version of JLine, there's a ton of nice little things now available in CommandBox 4.  The first is a totally revamped tab completion interface.  Pressing Tab is now prettier, colored, and more organized.  Help is integrated right into the interface, and pressing tab repeatedly will cycle through the available options instead of redrawing the screen over and over.

![](https://lh4.googleusercontent.com/CAV4bjlL2QXRkCdtA0i7EcobPmxjzUxJVuc8_yVLBVwnWNspmRXinj50krU6235KqqbQvvhcARP-4dkDUBCyUvsvWfMaXp_Hu-sThVUvQaaOGkd_FfabyfDatEL1uM1p0iA7hGeZZmY)

Next is color coding when you type commands in the shell.  This make it much easier to tell when you've typed the name of a command correct and makes the difference between the command and parameters easier on your eyes.

![](https://lh5.googleusercontent.com/AdTOYUgp4AHjIl_z8T74gcHUPnE_xltEcMQSHFQoy4J4amyN8pIOOofaPsgF4SRKsjXzK6dEXjJtYd2IPSB2qS3qv5aW-RJ74tQU8cvtrQ83tVDPQ-lRa1k8wqRMWMl3O0CUIxlDK1w)

Finally is tab complete and syntax highlighting in the REPL.  You can tab complete any CFML function as well as previous variable names you've typed.  Common CFML keywords are highlighted, as well as CFML functions and there's even color coded matching of braces, parens, and quotes as you type.

![](https://lh5.googleusercontent.com/CZMwrczMtdIO28zfybjTIdNA1RlBokYfEt8vOHq28-D3EKBbGhBRul-d4apkMwOMOZXghBwcGKwQx4oIUKWb_qE8WBHdKCbyZY5B1kOqvGwnClzc5VdRgtgI07bejO0KjsPh1DvZkDY)

### Shell History

There are two new features in the shell's history.  Pressing "up" will still show the previous items in your history.  But typing a partial command like "cd" and THEN hitting up will jump to the most recent histories that start with that word.  Very handy to find that one "coldbox create..." command you ran two days ago.

The second new history feature is known in the bash world as **i-search**.  Press **Ctrl-Shift-R** to open a search from the console where you can search your entire command history by keyword.  Keep pressing **Ctrl-Shift-R** to cycle backwards through the results.  Press **Ctrl-Shift-S** to cycle forwards through the results.  Press enter to run the matched search select, or edit it inline before running it.

### CommandBox Bullet Train

There's a new CommandBox module available called "**commandbox-bullet-train**" which makes the CLI look super sleek and sexy.  You can add it very easily with:

```bash
install commandbox-bullet-train
```

You'll want to install a powerline-patched font as well.  Check out the instructions under the "Fonts" section in the readme.

[https://www.forgebox.io/view/commandbox-bullet-train](https://www.forgebox.io/view/commandbox-bullet-train)

![](https://lh4.googleusercontent.com/ioB2v4uoj1OUxRnTB9iNSlENo1zaYbPjGMQq8MxJet1z0yfUMJ7J3pdZI6lVMYcu-He3Vx8gEmYEmguHHy2K9PRN3l2j1uucU24gqOKuqH46fc-zKzl_zNiXa1gfe3R_o0Ljy2dvtPM)

And is that some sweet new ASCII art taking advantage of 256 colors as well as a randomized quote/tip on every shell start?  Why yes, yes it is!

### Interactive Inputs

CommandBox has a new way to interact with users and give them a list of pre-defined options that doesn't require typing a free text response.  You'll see it if you try to start the same server twice in a row.  This is [fully documented](https://commandbox.ortusbooks.com/task-runners/task-interactivity#multiselect-input) and available for you to use in Task Runners and custom commands as well.

![](https://lh4.googleusercontent.com/leFNygdwIitqvz7eKLxP5N-t7_ogAuU57YDLhjSAHmoLYjnemjA17IcKIXqzQRu6i5adiO3Wr8bDKJA1gK3xk9shwRKejsqQP2EBprsStyFGGsODN6Vdai8nqM4z4k0b8bLFC6uIGjk)

### Interactive Jobs

Some of the more wordy tasks you perform like installing packages and starting servers have gotten a big makeover in how they reveal their output to you.  If an installation fails, you want to know about it, but so long as everything worked, you usually don't care.  These actions will now scroll the last few active log entries past in a controlled format, but hide them at the end so the shell stays much cleaner, even when installing dozens of packages at once. 

As an example, installing CFConfig actually installs 9 separate packages.  This used to output around 100 lines of console logging which no one in their right mind ever read.  All the same logging is still there, but now by the time it's done, this is all you see:

```text
> install commandbox-cfconfig --force
 ✓ | Installing package [forgebox:commandbox-cfconfig]
   | ✓ | Uninstalling package: commandbox-cfconfig
   | ✓ | Installing package [forgebox:cfconfig-services@be]
   |   | ✓ | Installing package [forgebox:lucee-password-util@^1.0.0]
   |   | ✓ | Installing package [forgebox:adobe-password-util@^1.0.0]
   |   |   | ✓ | Installing package [forgebox:propertyFile@^1.0.0]
   |   | ✓ | Installing package [forgebox:propertyFile@^1.0.7]
   |   | ✓ | Installing package [forgebox:semver@^1.0.0]
   |   | ✓ | Installing package [forgebox:JSONPrettyPrint@^1.2.6]
```

If you want to troubleshoot, or you are running this install as part of a build and you want to see all this output later, just use the **--verbose** flag.  For server starts, using the **--debug** flag will preserve all your precious log output on the screen after the server starts.

Even cooler, the Interactive Job interface is fully documented and available for you to use in your Task Runners or Custom Commands.  

Docs:

[https://commandbox.ortusbooks.com/task-runners/interactive-jobs](https://commandbox.ortusbooks.com/task-runners/interactive-jobs)

Example:

```javascript
job.start( 'Starting server' );
  job.addLog( 'This is the server name' );
  job.addWarnLog( 'Hey, don''t touch that dial' );
​
    job.start( 'Installing CF Engine first' );
      job.addLog( 'This was the version used' );
      job.addLog( 'Yeah, we''re done' );
    job.complete();
​
  job.addLog( 'Aaand, we''re back!.' );
  job.addErrorLog( 'I think we''re going to crash' );
​
job.error( 'Didn''t see that coming' );
```

Which looks like this when it's done:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LA-UVvV3_TgzQyCXMWK%2F-LCA6XEmeMpt5bi4OZnM%2F-LCA8qTM95eKrCM9YLWS%2Fimage.png?alt=media&token=bb2f09b3-d28e-4cb8-8bb4-5b5bb8abe650)

### Server Logs

We did a lot of work to make dealing with servers easier-- especially when it comes to your log files.  Console starts and tailing server logs are now color coded so it's easier to find errors and warnings.

![](https://lh4.googleusercontent.com/QffRjYN3ScVyNCZiysE9ocYIs7XHFuUXCBa940lObSKfnaq_Dyx0PpCVmXBrlLVxvSdzH9koVhzO5zg96KJlWy3VLa-DfolsnmwcOEjUMgzQj67RnL4uk96mSV7FaYLSeQVylq-X_9w)

We've also fine tuned what information shows up when you do **--console** starts as well as **--debug** starts to reduce the noise and enhance the useful information.  For instance, when you do:

```bash
server start --rewritesEnable --console --debug
```

You'll see a line of debug logging that shows if the URL rewrites kicked in and what the URL was rewritten to.  How useful is that?!

Remember you can view and tail the server "out" logs like so:

```bash
server log
server log --follow
```

### Access Logs

The built in Undertow web server that CommandBox uses just got more powerful.  You can turn on access logs that show you every incoming HTTP request in the same "common format" as Apache web server.  

```bash
server set web.accessLogEnable=true
```

You can view and tail this log file like so:

```bash
server log --access
server log --access --follow
```

### URL Rewrite Logs

But wait, there's more logging goodness.  Troubleshooting rewrite rules can be really tricky.  That's why we broke out a new separate log file just for Tuckey Rewrites to dump into.  You can dial in how much information you get with **--debug** and **--trace** server starts.

```bash
server set web.rewrites.logEnable=true
```

You can view and tail this log file like so:

```bash
server log --rewrites
server log --rewrites --follow
```

### Automatic Log Rotation

CommandBox web servers are truly ready for prime time.  All the Undertow log files above automatically rotate which means you'll never fill up a hard drive on accident due to out of control log files.

### Version Checks on Startup

Another optional module you can install is the CommandBox update check module.  It will check every 24 hours \(when starting the shell\) and let you know if your CLI or any of your system modules are out of date.

```bash
install commandbox-update-check
```

### Pipe output of native OS binaries

This used to work back in the day, but was a regression back when I added the ability to interact with native binaries.  Now you have the best of both worlds.

```bash
 echo "java -version" | run | #ucase
```

### Task DSL

Running other tasks from inside of Task Runners is now easier.  Docs:

[https://commandbox.ortusbooks.com/task-runners/running-other-tasks](https://commandbox.ortusbooks.com/task-runners/running-other-tasks)

Example:

```javascript
task( 'build' )
    .run();
```

_\(Same as running "task run build" from the CLI\)_

### Actual Proper Non-Sucking Ctrl-C and Ctrl-D support

You can now cancel long running commands, tasks, and even HTTP downloads by pressing **Ctrl-C**.  Yay!  Pressing **Ctrl-C** from the prompt does nothing, which is consistent with other shells.  Pressing **Ctrl-D** from the shell will now exit CommandBox entirely which is also consistent with other shells.  In case you're wondering, **Ctrl-C** fires the interrupt terminal signal, and **Ctrl-D** sends the EOF \(end of file\) signal.

Docs:

[https://commandbox.ortusbooks.com/usage/interactive-shell-features\#ctrl-c-and-ctrl-d](https://commandbox.ortusbooks.com/usage/interactive-shell-features#ctrl-c-and-ctrl-d)

### Load ad-hoc jars for Task Runners

You can now load ad-hoc jars right from Task Runners which is sometimes necessary for working with Java libs.  Docs:

[https://commandbox.ortusbooks.com/task-runners/loading-ad-hoc-jars](https://commandbox.ortusbooks.com/task-runners/loading-ad-hoc-jars)

Examples:

```javascript
classLoad( 'D:/amqp-client-5.1.2.jar' );
classLoad( 'C:/myLibs,C:/otherLibs' );
classLoad( [ 'C:/myLibs', 'C:/otherLibs' ] );
classLoad( 'C:/myLibs/myLib.jar,C:/otherLibs/other.class' );
classLoad( [ 'C:/myLibs/myLib.jar', 'C:/otherLibs/other.class' ] );
```

### Task Scaffolding

Wanted to play with Task Runners but not sure where you start?  Drop everything, grab the closest CommandBox 4 CLI, and run these two commands:

```bash
task create --open
task run
```

You just created a new task and ran it.  Go on, look around!  

[https://commandbox.ortusbooks.com/task-runners/task-anatomy](https://commandbox.ortusbooks.com/task-runners/task-anatomy)

### Updated Directory Listing

Directory listings have gotten a makeover.  The columns actually align, the file sizes are human-readable, and the file types are color coded.  Be careful, you might actually be able to find stuff now!

![](https://lh3.googleusercontent.com/CeAwNtJkNjrGFk1EnGp9r6iSRedBJih2B_YpLN-VHy_HOaxmJfDKWXkz9TwkYHsEIDkRHpvU29E7Q9xPhOu1DeuA3-wwMyCNOwmj8DzLBZHuTGOb3yeaJGaAj8iU-W0HZhjYifDGLPc)

### ASCII Art Stereograms

If you remember the "Magic Eye" books from your childhood, you'll be pleased to know CommandBox has an ASCII Art Stereogram for every day of the month.  You'll find it hiding inside the **info** command.  The "image" will change every day at midnight.

```text
    _( )          _( )         _( )          _( )        _( )
  _( )  )_      _( )  )_     _( )  )_      _( )  )_    _( )  )_
 (____(___)    (____(___)   (____(___)    (____(___)  (____(___)
​
​
   /\          /\           /\          /\         /\
  /  \  /\    /  \  /\     /  \  /\    /  \  /\   /  \  /\
 /    \/  \  /    \/  \   /    \/  \  /    \/  \ /    \/  \
           \/          \ /          \/          /          \/
   ..        ..        ..         ..        ..         ..
"        "         "        "         "         "        "
    *       *        *       *        *       *       *       *
  @     @      @     @      @      @     @      @     @     @
 \|/   \|/    \|/   \|/    \|/    \|/   \|/    \|/   \|/   \|/
```

If you keep looking like that, your face will freeze that way!

## Known Breaking Changes

We tried very hard to keep CommandBox 4 compatible but there are a few things that might surprise you.

* The REPL and Task Runners run against Lucee **5.2.7** instead of  **4.5.5**.  That might affect valid CFML syntax as well as datasource definitions
* The default server you get when you type "**server start**" is also Lucee **5.2.7**, not Luce **4.5.5**.
* **Java 7** support removed.  This affects both the core CLI as well as any servers.  For CF9 users, you can still run CF9 servers but you'll need to use an older version of Java 8 such as **1.8.0\_92.**  _\(Note: Java 9 and 10 don't work yet!\)_
* Native CFML execution via **box foo.cfm** now routes through the "**execute**" command which means no **Application.cfc** will get run.  You can refactor your cfm scripts or use the undocumented **\_internalRequest\(\)** function in Lucee 5.
* You no longer can use **\t** and **\n** to escape tab and line breaks in command parameters. This caused a lot of confusion in Windows paths and there are other ways to do it right in your terminal.  Check out the [docs on it.](https://commandbox.ortusbooks.com/usage/parameters/escaping-special-characters#line-breaks)
* The **waitForKey\(\)** method in Task Runners and custom commands no longer returns the ASCII code, but the actual character pressed OR a special string representing the key press like "**key\_up**" or "**key\_down**".  Check out the [docs here](https://commandbox.ortusbooks.com/task-runners/task-interactivity#waitforkey).
* **CommandBox 4** is prettier, more productive, and cooler than CommandBox 3.  This may cause CLI envy with your Node coworkers.  Don't worry, this is normal.  



### Bug

* \[[COMMANDBOX-174](https://ortussolutions.atlassian.net/browse/COMMANDBOX-174)\] - Box CLI not working inside cygwin
* \[[COMMANDBOX-395](https://ortussolutions.atlassian.net/browse/COMMANDBOX-395)\] - Commandbox 3.1.X no longer works with Git Bash
* \[[COMMANDBOX-728](https://ortussolutions.atlassian.net/browse/COMMANDBOX-728)\] - Allow control of default package name when box.json is missing
* \[[COMMANDBOX-749](https://ortussolutions.atlassian.net/browse/COMMANDBOX-749)\] - Server won't start with $ in web root path
* \[[COMMANDBOX-750](https://ortussolutions.atlassian.net/browse/COMMANDBOX-750)\] - Can't list files in directory with parenthesis in the name
* \[[COMMANDBOX-761](https://ortussolutions.atlassian.net/browse/COMMANDBOX-761)\] - Default rewrites don't start regex at the start of the request URI
* \[[COMMANDBOX-763](https://ortussolutions.atlassian.net/browse/COMMANDBOX-763)\] - Ctrl-C in shell kills associated server processes on \*nix
* \[[COMMANDBOX-767](https://ortussolutions.atlassian.net/browse/COMMANDBOX-767)\] - Issue installing older CF engine when two versions exist who only differ in build ID
* \[[COMMANDBOX-778](https://ortussolutions.atlassian.net/browse/COMMANDBOX-778)\] - Adobe war has incorrect default /CFIDE CF mapping
* \[[COMMANDBOX-782](https://ortussolutions.atlassian.net/browse/COMMANDBOX-782)\] - CLI Loader crashes: Error reloading cached bundle
* \[[COMMANDBOX-783](https://ortussolutions.atlassian.net/browse/COMMANDBOX-783)\] - ls and dir do not list directory content after 'cd ..' without trailing slash
* \[[COMMANDBOX-785](https://ortussolutions.atlassian.net/browse/COMMANDBOX-785)\] - restart command not correctly detecting stopped server
* \[[COMMANDBOX-787](https://ortussolutions.atlassian.net/browse/COMMANDBOX-787)\] - Starting two servers at once can corrupt servers.json file
* \[[COMMANDBOX-788](https://ortussolutions.atlassian.net/browse/COMMANDBOX-788)\] - Starting server from non-ForgeBox endpoint doesn't detect proper engine/verion
* \[[COMMANDBOX-790](https://ortussolutions.atlassian.net/browse/COMMANDBOX-790)\] - Package publishing fails with folder named "readme" in the root

### Story

* \[[COMMANDBOX-724](https://ortussolutions.atlassian.net/browse/COMMANDBOX-724)\] - Control HTTPOnly and secure attribute of JSESSIONID

### New Feature

* \[[COMMANDBOX-73](https://ortussolutions.atlassian.net/browse/COMMANDBOX-73)\] - Version check on startup
* \[[COMMANDBOX-566](https://ortussolutions.atlassian.net/browse/COMMANDBOX-566)\] - CommandBox bullet train
* \[[COMMANDBOX-583](https://ortussolutions.atlassian.net/browse/COMMANDBOX-583)\] - Create a "checkbox" user input for commands
* \[[COMMANDBOX-722](https://ortussolutions.atlassian.net/browse/COMMANDBOX-722)\] - Task DSL
* \[[COMMANDBOX-725](https://ortussolutions.atlassian.net/browse/COMMANDBOX-725)\] - Make SSL work on Adobe servers
* \[[COMMANDBOX-726](https://ortussolutions.atlassian.net/browse/COMMANDBOX-726)\] - Allow testbox run runner to be relative URL
* \[[COMMANDBOX-727](https://ortussolutions.atlassian.net/browse/COMMANDBOX-727)\] - validate box.json properties for testbox run usage
* \[[COMMANDBOX-729](https://ortussolutions.atlassian.net/browse/COMMANDBOX-729)\] - Change default jAnsi temp path
* \[[COMMANDBOX-745](https://ortussolutions.atlassian.net/browse/COMMANDBOX-745)\] - Updating ColdBox commands to ColdBox 5
* \[[COMMANDBOX-751](https://ortussolutions.atlassian.net/browse/COMMANDBOX-751)\] - Enhance REPL console highlighter to work with parens and curlys
* \[[COMMANDBOX-752](https://ortussolutions.atlassian.net/browse/COMMANDBOX-752)\] - Support 256 colors with print helper
* \[[COMMANDBOX-757](https://ortussolutions.atlassian.net/browse/COMMANDBOX-757)\] - Update testbox command to trim and prettify json results
* \[[COMMANDBOX-759](https://ortussolutions.atlassian.net/browse/COMMANDBOX-759)\] - Be able to add jars to core Lucee classloader from inside the CLI
* \[[COMMANDBOX-760](https://ortussolutions.atlassian.net/browse/COMMANDBOX-760)\] - Command to scaffold new task
* \[[COMMANDBOX-771](https://ortussolutions.atlassian.net/browse/COMMANDBOX-771)\] - Create dedicated log for rewrites

### Improvement

* \[[COMMANDBOX-438](https://ortussolutions.atlassian.net/browse/COMMANDBOX-438)\] - Remember the currently edited command when navigating through the history
* \[[COMMANDBOX-482](https://ortussolutions.atlassian.net/browse/COMMANDBOX-482)\] - better tab completion for REPL
* \[[COMMANDBOX-527](https://ortussolutions.atlassian.net/browse/COMMANDBOX-527)\] - Upgrade to JLine3
* \[[COMMANDBOX-552](https://ortussolutions.atlassian.net/browse/COMMANDBOX-552)\] - Update Launch4j library
* \[[COMMANDBOX-596](https://ortussolutions.atlassian.net/browse/COMMANDBOX-596)\] - Refactor \`Box foo.cfm\` to funnel through execute command
* \[[COMMANDBOX-702](https://ortussolutions.atlassian.net/browse/COMMANDBOX-702)\] - Remove \t and \n escapes
* \[[COMMANDBOX-706](https://ortussolutions.atlassian.net/browse/COMMANDBOX-706)\] - Upgrade CLI core to use Lucee 5
* \[[COMMANDBOX-714](https://ortussolutions.atlassian.net/browse/COMMANDBOX-714)\] - Parsing issue with native OS binaries
* \[[COMMANDBOX-719](https://ortussolutions.atlassian.net/browse/COMMANDBOX-719)\] - Add serverDetails and installDetails to the onServerStart interceptor
* \[[COMMANDBOX-720](https://ortussolutions.atlassian.net/browse/COMMANDBOX-720)\] - Add web access logs to undertow
* \[[COMMANDBOX-730](https://ortussolutions.atlassian.net/browse/COMMANDBOX-730)\] - Improve message on server forget
* \[[COMMANDBOX-731](https://ortussolutions.atlassian.net/browse/COMMANDBOX-731)\] - Don't escape params\(\) in CommandDSL when the command is "run"
* \[[COMMANDBOX-735](https://ortussolutions.atlassian.net/browse/COMMANDBOX-735)\] - Change how Ctrl-C and Ctrl-D behave
* \[[COMMANDBOX-736](https://ortussolutions.atlassian.net/browse/COMMANDBOX-736)\] - Pressing "up" filters history on what you've already typed
* \[[COMMANDBOX-737](https://ortussolutions.atlassian.net/browse/COMMANDBOX-737)\] - Allow Ctrl-C to interrupt executing tasks like downloading a file
* \[[COMMANDBOX-738](https://ortussolutions.atlassian.net/browse/COMMANDBOX-738)\] - REPL isn't clear whether expression returned empty string or null
* \[[COMMANDBOX-739](https://ortussolutions.atlassian.net/browse/COMMANDBOX-739)\] - Allow commands to be interruptible with Ctrl-C
* \[[COMMANDBOX-740](https://ortussolutions.atlassian.net/browse/COMMANDBOX-740)\] - Highlight code in the repl
* \[[COMMANDBOX-741](https://ortussolutions.atlassian.net/browse/COMMANDBOX-741)\] - Add prePrompt interception point
* \[[COMMANDBOX-742](https://ortussolutions.atlassian.net/browse/COMMANDBOX-742)\] - Allow installPath to override PackageDirectory
* \[[COMMANDBOX-744](https://ortussolutions.atlassian.net/browse/COMMANDBOX-744)\] - preProcessLine and postProcessLine interception points
* \[[COMMANDBOX-747](https://ortussolutions.atlassian.net/browse/COMMANDBOX-747)\] - Allow raw params to CommandDSL that aren't escaped
* \[[COMMANDBOX-753](https://ortussolutions.atlassian.net/browse/COMMANDBOX-753)\] - start --console should exit if server is killed externally
* \[[COMMANDBOX-754](https://ortussolutions.atlassian.net/browse/COMMANDBOX-754)\] - Handle download progress when no total file size is avaiable
* \[[COMMANDBOX-755](https://ortussolutions.atlassian.net/browse/COMMANDBOX-755)\] - Switch to load CFML engine via JSR-223
* \[[COMMANDBOX-756](https://ortussolutions.atlassian.net/browse/COMMANDBOX-756)\] - Throw on invalid server.json
* \[[COMMANDBOX-758](https://ortussolutions.atlassian.net/browse/COMMANDBOX-758)\] - Add rewrite exception for Adobe CF's cf\_scripts folder
* \[[COMMANDBOX-764](https://ortussolutions.atlassian.net/browse/COMMANDBOX-764)\] - Allow output of native OS binaries to be captured from CLI and task runners
* \[[COMMANDBOX-765](https://ortussolutions.atlassian.net/browse/COMMANDBOX-765)\] - Upgrade to latest JGit lib
* \[[COMMANDBOX-768](https://ortussolutions.atlassian.net/browse/COMMANDBOX-768)\] - PackageDirectory in package box.json is never honored
* \[[COMMANDBOX-769](https://ortussolutions.atlassian.net/browse/COMMANDBOX-769)\] - Improve "testbox run" error output on Adobe CF
* \[[COMMANDBOX-770](https://ortussolutions.atlassian.net/browse/COMMANDBOX-770)\] - Update bundled JRE to latest
* \[[COMMANDBOX-773](https://ortussolutions.atlassian.net/browse/COMMANDBOX-773)\] - Upgrade to WireBox 5.0
* \[[COMMANDBOX-774](https://ortussolutions.atlassian.net/browse/COMMANDBOX-774)\] - Cache CFC metadata for faster startup times
* \[[COMMANDBOX-775](https://ortussolutions.atlassian.net/browse/COMMANDBOX-775)\] - Refresh progress bar UI
* \[[COMMANDBOX-776](https://ortussolutions.atlassian.net/browse/COMMANDBOX-776)\] - UI control for "Jobs" to pare down output for several operations
* \[[COMMANDBOX-777](https://ortussolutions.atlassian.net/browse/COMMANDBOX-777)\] - Spruce up info command with easter eggs
* \[[COMMANDBOX-780](https://ortussolutions.atlassian.net/browse/COMMANDBOX-780)\] - Spruce up dir command
* \[[COMMANDBOX-786](https://ortussolutions.atlassian.net/browse/COMMANDBOX-786)\] - Default to latest in upgrade command when on a prerelease already
* \[[COMMANDBOX-789](https://ortussolutions.atlassian.net/browse/COMMANDBOX-789)\] - Add additional debugging information to the "info" command



