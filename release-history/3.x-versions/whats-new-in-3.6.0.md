# What's New in 3.6.0

### Improve OS binary execution

You've been able to [run OS binaries](https://commandbox.ortusbooks.com/content/usage/execution/os_binaries.html) from the CommandBox interactive shell for a while now which is great for adding native CLI calls to your recipes.

```bash
CommandBox> !git push
```

The biggest problem with this though was that no output shows on the screen until the OS command is completely finished and if your OS command blocks for interactivity or just never ends, the CommandBox shell will just "hang" with no output.  All that is a thing of the past now.  The standard input and output of the OS process is now bound to the standard input and output of your CommandBox shell.  That means that you see output as soon as the binary outputs it and if it stops to ask you for input, you can provide it.  This opens up a world of possibilities.  

* You can ping an address and watch the output stream as the packets return.
* You can do a Git commit and interact with the VI window that appears to capture your commit message.
* You can actually open a bash/DOS shell right inside of CommandBox and then "exit" back to box when you're done.
* Run native commands that collect input from you to continue like a sudo password.

```bash
CommandBox> !ping -c 4 google.com
CommandBox> !git commit
CommandBox> !bash
CommandBox> !sudo command
```

This has been tested and works pretty well on Mac and Windows.  Note, we've seen some issues on Linux where output is streamed, but input is not captured.

### New interceptor points

* [**preServerForget**](https://commandbox.ortusbooks.com/content/developing/interceptors/core/server_lifecycle.html) - Always fires before attempting to forget a server whether or not the forgetting is actually successful
* [**postServerForget**](https://commandbox.ortusbooks.com/content/developing/interceptors/core/server_lifecycle.html) - Fires after a successful server forget. If the forget fails, this will not fire.

### Allow spaces in user home directory

This was supported in CommandBox 3.4.0, but we had a regression in 3.5.0 that caused issues for users who have a space in their path to CommandBox's home dir.  This has been fixed along with some related issues with the FusionReactor module.  Make sure you have the latest Fusion Reactor module once you upgrade to CommandBox 3.6.0.

```bash
CommandBox> install commandbox-fusionreactor
```

### New Lucee version

The core version of Lucee that the CLI runs on has been updated to **4.5.5.006**.  Please note this means the default version of Lucee that starts up for your server when you don't specify otherwise will change.  If you have settings like datasources and such that you want to keep, make sure you lock in your exact version of Lucee or check into our new CFConfig project for exporting/importing your server settings.

### Incorrect exit code from "testbox run"

The **testbox run** command would return an exit code of 0 when your tests had in fact failed.  This has been fixed so you can trust a proper exit code from the process when running your tests inside a Jenkins or Travis CI build.

```bash
$> box testbox run runner=http://localhost:8080/tests/runner.cfm
```

### Global default for HTTP port in config settings

Previously, you couldn't set a global default HTTP port for all your servers.  This was on purpose since it didn't really make sense since one one server can use a port at a time.  Now, with the introduction of Chris Schmitz's host updater module, you can more easily run each server on a dedicated host name which is added to your host file for you and bound to a unique port.  This allows you to run all your local servers on port 80 which is great for cleaning up your local dev.  As such, we've added the ability to set the global HTTP port now in your config setting's server defaults.

```text
CommandBox> config set server.defaults.web.http.port=80
```

## Release Notes

Here's the full release notes for the **3.6.0** release.

### Bug

* \[[COMMANDBOX-553](https://ortussolutions.atlassian.net/browse/COMMANDBOX-553)\] - Windows CommandBox upgrades fail silently if servers are left running
* \[[COMMANDBOX-554](https://ortussolutions.atlassian.net/browse/COMMANDBOX-554)\] - Box start error
* \[[COMMANDBOX-555](https://ortussolutions.atlassian.net/browse/COMMANDBOX-555)\] - All semicolons removed in REPL which breaks some code
* \[[COMMANDBOX-558](https://ortussolutions.atlassian.net/browse/COMMANDBOX-558)\] - Can't start server when space is in user home dir
* \[[COMMANDBOX-559](https://ortussolutions.atlassian.net/browse/COMMANDBOX-559)\] - server list --verbose produces an error
* \[[COMMANDBOX-567](https://ortussolutions.atlassian.net/browse/COMMANDBOX-567)\] - package list sometimes shows incorrect version of 1.0.0
* \[[COMMANDBOX-572](https://ortussolutions.atlassian.net/browse/COMMANDBOX-572)\] - Running server info on non-server folder creates empty server details
* \[[COMMANDBOX-575](https://ortussolutions.atlassian.net/browse/COMMANDBOX-575)\] - CommandBox fails to start if a 3rd party module fails to load
* \[[COMMANDBOX-582](https://ortussolutions.atlassian.net/browse/COMMANDBOX-582)\] - NPE on some URLs occasionally
* \[[COMMANDBOX-587](https://ortussolutions.atlassian.net/browse/COMMANDBOX-587)\] - testbox run doesn't always return correct exit code on failure

### Improvement

* \[[COMMANDBOX-502](https://ortussolutions.atlassian.net/browse/COMMANDBOX-502)\] - improve execution of OS binaries in "run"
* \[[COMMANDBOX-556](https://ortussolutions.atlassian.net/browse/COMMANDBOX-556)\] - Add xxxServerForget interceptors to the Server lifecycle
* \[[COMMANDBOX-570](https://ortussolutions.atlassian.net/browse/COMMANDBOX-570)\] - Improve port binding detection
* \[[COMMANDBOX-571](https://ortussolutions.atlassian.net/browse/COMMANDBOX-571)\] - Allow port to be defaulted in config settings
* \[[COMMANDBOX-576](https://ortussolutions.atlassian.net/browse/COMMANDBOX-576)\] - Improve CommandBox module installations
* \[[COMMANDBOX-578](https://ortussolutions.atlassian.net/browse/COMMANDBOX-578)\] - Bump Lucee version to 4.5.5.006

