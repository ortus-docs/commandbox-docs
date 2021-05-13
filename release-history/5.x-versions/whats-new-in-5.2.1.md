# What's New in 5.2.1

There is now a new "forgebox logout" command you can use for testing or just to remove your API token from the local CLI.

```bash
# Logout one user
forgebox logout username

# logout all users
forgebox logout
```

You can change CommandBox's default tab completion to be an inline list that follows your cursor. **This setting requires you to close and re-open the shell to take affect.**

```bash
config set tabCompleteInline=true
```

![](https://www.ortussolutions.com/__media/CLIlistcomplete.png)

Read more here: 

[https://commandbox.ortusbooks.com/config-settings/misc-settings\#tabcompleteinline](https://commandbox.ortusbooks.com/config-settings/misc-settings#tabcompleteinline)

We've added better debugging information for Server Profiles.  If you add the **--verbose** flag to your server start, you'll be able to see what profile was detected for your server, and what baked-in rules have been turned on as a result.

```text
|------------------------------
   | √ | Setting Server Profile to [development]
   |   |------------------------------------------------------
   |   | Profile set from "environment" env var
   |   | Block CF Admin disabled
   |   | Block Sensitive Paths enabled
   |   | Block Flash Remoting enabled
   |   | Directory Browsing enabled
   |   |------------------------------------------------------
```

We've added a new **Single Server Mode** you can enable in the CLI to make using CommandBox in Docker images easier.

Read more here:

[https://commandbox.ortusbooks.com/embedded-server/single-server-mode](https://commandbox.ortusbooks.com/embedded-server/single-server-mode)

## Release Notes

Here are the release notes for the 5.2.1 release.

### Bug

* \[[COMMANDBOX-1231](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1231)\] - Installing via lex endpoint uses incorrect file extension
* \[[COMMANDBOX-1232](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1232)\] - Location of predicate file is in a folder that the Docker finalization script deletes
* \[[COMMANDBOX-1238](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1238)\] - Command alas for run command doesn't expand properly

### New Feature

* \[[COMMANDBOX-1237](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1237)\] - Add config setting to activate JLine's AUTO\_MENU\_LIST
* \[[COMMANDBOX-1240](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1240)\] - forgebox logout command

### Improvement

* \[[COMMANDBOX-1227](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1227)\] - verbose server start output for profile and security settings
* \[[COMMANDBOX-1228](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1228)\] - Extend ${} scopes to apply to any getSetting\(\) call or "env show" command
* \[[COMMANDBOX-1229](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1229)\] - Modules aren't unloaded on reload or shutdown
* \[[COMMANDBOX-1233](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1233)\] - Add debug output that shows location of commandbox.properties file on start
* \[[COMMANDBOX-1234](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1234)\] - Add single server mode for CommandBox in a Docker container
* \[[COMMANDBOX-1236](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1236)\] - Add Testbox runner to sensitive paths in production profile
* \[[COMMANDBOX-1241](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1241)\] - Use UTF-8 when reading files with "cat" command

