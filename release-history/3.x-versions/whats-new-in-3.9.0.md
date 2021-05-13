# What's New in 3.9.0

### UNC Network path support

This has been pretty big for Windows users who access files on their servers over a UNC network path like **\\server-name\foo\bar**.  You can now **cd** into paths starting with \\, perform file operations like **cat** against those paths, etc.  Note backslashes need escaped in the CommandBox shell.

```bash
CommandBox> cd \\\\server-name/share
CommandBox> cd \\\\192.168.123.105/share
```

This was pretty straightforward since Java already supports this, but I had to change a lot of core path handling to make sure the correct slashes were preserved.  This needs a fair amount of testing to make sure we nailed it down for good.  If your network share requires permissions, you'll need to have saved those in Windows Explorer already  or execute a "net use" OS command from the shell.

### Task Runner Improvements

When running a task from the CLI, the user will be automatically prompted if they don't supply all the required args.  This is just like commands work now.

```bash
CommandBox> task run myTask
Please enter required field "Foo": _
```

We also fixed several bugs with passing positional parameters and flag to task runners.  

```bash
# positional
CommandBox> task run taskFileName targetName value1 value2 true
# named
CommandBox> task run taskFile=taskFileName target=targetName :param1=value1 :param2=value2 :param3=true
# Use a Flag
CommandBox> task run taskFileName targetName value1 value2 --:param3
CommandBox> task run taskFile=taskFileName target=targetName :param1=value1 :param2=value2 --:param3
```

### "package link" and "package unlink" for module development

This has been a long time coming, but if you want to work on a CommandBox module now, you don't have to keep copying files over to your CommandBox installation just to test. Instead, just run this from the root of your module's repo:

```bash
CommandBox> package link
```

This will symlink \(works on Windows and \*nix\) your module into the core CLI's modules folder and reload the shell so you can immediately start testing.  When you're done, just run **package unlink**.  If you'd like to use this same feature, but to link a ColdBox module's repo over to a test application so you can test it without making a copy, you can pass in the path to the remote modules folder you'd like to link to.

```bash
CommandBox> package link /path/to/test/app/modules
```

This is a little easier than using your OS's native symlink commands and can be used in recipes that will work across operating systems!

### Everything Else \(mostly\)

The rest of the changes don't really need a dedicated section but they're worth mentioning, so I've put them in this tidy list :\)  If you'd like the ticket numbers, you can get them out of this full list of tickets in JIRA: [https://ortussolutions.atlassian.net/secure/ReleaseNote.jspa?projectId=11000&version=20400](https://ortussolutions.atlassian.net/secure/ReleaseNote.jspa?projectId=11000&version=20400)

* **box install** returns failing exit code if a package install fails. This helps builds fail correctly if things go wrong
* When prompted to type in something like a missing parameter, your answer is no longer added to the command history
* The **--debug** flag works correctly when starting a server from your OS shell like **$&gt; box server start --debug**
* **box.json** dependencies are stored with forward slashes so Mac and Windows devs stop fighting over which file to commit
* **config set** no longer prints out the value to avoid leaking secrets in build script output.
* Updating a package now uninstalls the previous version first to ensure a fresh start since the new version may have removed files.
* If you're setting CommandBox behind an AJP proxy, we exposed the flag and ports for that as first class citizens of **server.json**.
* Visual markers for private packages in the **forgebox search** command.
* The **package init** command creates a valid slug for private packages in the **@user/slug** format.
* [Command parameters defaults](https://ortus.gitbooks.io/commandbox-documentation/content/usage/execution/default-command-parameters.html) work on all the aliases for a command now.
* New **--local** flag to **server list** to show all servers that have been started in the current working directory
* If for some reason you want to supply some ad-hoc JVM args to the actual CLI process, you can create a new environment var called **BOX\_JAVA\_PROPS="foo=bar;brad=wood"**
* You can now **touch** files in a non-existent directory and it will create the directory instead of erroring.
* Viewing a ForgeBox package via **package show** with a markdown based description, now has basic formatting in the CLI
* The default URL rewrite file doesn't try to rewrite requests to **/favicon.ico** even when it doesn't exist.
* Our CF11 servers no longer have secure profile enabled. That was causing issues due to some of the settings like returning 200 on error.   If you were making use of that default, please use [CFConfig](https://cfconfig.ortusbooks.com/) to set what you need.
* At John Farrar's request, several URLs in output messages have had space put before and after them so capable shells will auto-link them correctly. 
* Improved the Java networking error messages on server start if the host name wasn't correct in your host file and you were letting CommandBox pick a random port for you.
* Prevented unnecessary saves to **box.json** when installing to keep file updated dates from being touched.
* Added friendly check for Java 9 since it's not supported yet and the error that displayed made zero sense.
* Commands like **forgebox show** and **forgebox list** now can provide their data in JSON format. ex: **forgebox show coldbox --json**

\*\*\*\*

**Bug**

* \[[COMMANDBOX-579](https://ortussolutions.atlassian.net/browse/COMMANDBOX-579)\] - --debug flag is eaten when running CommandBox from native OS
* \[[COMMANDBOX-640](https://ortussolutions.atlassian.net/browse/COMMANDBOX-640)\] - Ensure clean install/update of packages
* \[[COMMANDBOX-681](https://ortussolutions.atlassian.net/browse/COMMANDBOX-681)\] - Touching file in nonexistent directory errors instead of creating directory
* \[[COMMANDBOX-682](https://ortussolutions.atlassian.net/browse/COMMANDBOX-682)\] - CommandDSL that errors out doesn't reset CWD
* \[[COMMANDBOX-684](https://ortussolutions.atlassian.net/browse/COMMANDBOX-684)\] - positional task args don't work
* \[[COMMANDBOX-686](https://ortussolutions.atlassian.net/browse/COMMANDBOX-686)\] - cp command doesn't work for folders
* \[[COMMANDBOX-689](https://ortussolutions.atlassian.net/browse/COMMANDBOX-689)\] - CommandBox Modules customInterceptionPoints can't accept an array
* \[[COMMANDBOX-690](https://ortussolutions.atlassian.net/browse/COMMANDBOX-690)\] - CommandBox has no \`processState\` method on the InterceptorService
* \[[COMMANDBOX-691](https://ortussolutions.atlassian.net/browse/COMMANDBOX-691)\] - Flags aren't passed correctly to task runners
* \[[COMMANDBOX-701](https://ortussolutions.atlassian.net/browse/COMMANDBOX-701)\] - CFML functions don't handle incoming JSON with pound signs

**New Feature**

* \[[COMMANDBOX-653](https://ortussolutions.atlassian.net/browse/COMMANDBOX-653)\] - Expose Runwar AJP listener settings
* \[[COMMANDBOX-663](https://ortussolutions.atlassian.net/browse/COMMANDBOX-663)\] - Update server list and server info to be able to show all the servers on a particular directory
* \[[COMMANDBOX-668](https://ortussolutions.atlassian.net/browse/COMMANDBOX-668)\] - New package link and package unlink commands
* \[[COMMANDBOX-680](https://ortussolutions.atlassian.net/browse/COMMANDBOX-680)\] - Add ad-hoc JVM props via an environment variable

**Improvement**

* \[[COMMANDBOX-178](https://ortussolutions.atlassian.net/browse/COMMANDBOX-178)\] - Don't store text entered to "ask\(\)" command in history
* \[[COMMANDBOX-565](https://ortussolutions.atlassian.net/browse/COMMANDBOX-565)\] - Handle minor version updating a bit better
* \[[COMMANDBOX-607](https://ortussolutions.atlassian.net/browse/COMMANDBOX-607)\] - Always store dependency install paths with forward slashes
* \[[COMMANDBOX-609](https://ortussolutions.atlassian.net/browse/COMMANDBOX-609)\] - Have a setting to not show secrets when printing out the config
* \[[COMMANDBOX-624](https://ortussolutions.atlassian.net/browse/COMMANDBOX-624)\] - Support UNC file paths on Windows
* \[[COMMANDBOX-639](https://ortussolutions.atlassian.net/browse/COMMANDBOX-639)\] - JSON format for forgebox endpoints
* \[[COMMANDBOX-659](https://ortussolutions.atlassian.net/browse/COMMANDBOX-659)\] - Ask user for required params to task runners
* \[[COMMANDBOX-660](https://ortussolutions.atlassian.net/browse/COMMANDBOX-660)\] - Visually show if a package is private when listing or showing
* \[[COMMANDBOX-661](https://ortussolutions.atlassian.net/browse/COMMANDBOX-661)\] - make package init create correct slug for private package
* \[[COMMANDBOX-662](https://ortussolutions.atlassian.net/browse/COMMANDBOX-662)\] - Make default command parms work on aliases
* \[[COMMANDBOX-670](https://ortussolutions.atlassian.net/browse/COMMANDBOX-670)\] - Box install failures to produce non-zero exit codes so build fails instead of continuing installation.
* \[[COMMANDBOX-683](https://ortussolutions.atlassian.net/browse/COMMANDBOX-683)\] - Provide ANSI formatting for markdown package descriptions
* \[[COMMANDBOX-685](https://ortussolutions.atlassian.net/browse/COMMANDBOX-685)\] - box.json template isn't proper JSON
* \[[COMMANDBOX-687](https://ortussolutions.atlassian.net/browse/COMMANDBOX-687)\] - Remove background color from CommandBox ASCII art
* \[[COMMANDBOX-688](https://ortussolutions.atlassian.net/browse/COMMANDBOX-688)\] - Default rewrite rules to ignore favicon.ico
* \[[COMMANDBOX-694](https://ortussolutions.atlassian.net/browse/COMMANDBOX-694)\] - Disable Secure Profile on CFEngine WARs
* \[[COMMANDBOX-696](https://ortussolutions.atlassian.net/browse/COMMANDBOX-696)\] - Leave space around URLs so some consoles will be clickable
* \[[COMMANDBOX-697](https://ortussolutions.atlassian.net/browse/COMMANDBOX-697)\] - Improve performance of package install ignores
* \[[COMMANDBOX-700](https://ortussolutions.atlassian.net/browse/COMMANDBOX-700)\] - Improve error message in ServerService.getRandomPort\(\)
* \[[COMMANDBOX-707](https://ortussolutions.atlassian.net/browse/COMMANDBOX-707)\] - Prevent unnecessary writes to box.json file when installing dependencies
* \[[COMMANDBOX-708](https://ortussolutions.atlassian.net/browse/COMMANDBOX-708)\] - Improve formatting when asking for required param that has no hint
* \[[COMMANDBOX-712](https://ortussolutions.atlassian.net/browse/COMMANDBOX-712)\] - Add Java 9 check to CommandBox until its supported

