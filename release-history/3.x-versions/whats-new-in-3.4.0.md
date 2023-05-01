# What's New in 3.4.0

## Bug Fixes

This release fixes an issue where Adobe CF servers will not start if you're machine is offline and also fixes a bug where the previous version of CommandBox didn't correctly remove old versions of jar files on upgrade.

## Enhancements

Git tags when bumping a package command can have a custom prefix now.  Tab completion options are also alphabetized.  Ctrl-C is also handled better on Unix and actually works in Windows!  Also, the timestamp on your sever.json file won't be updated unless the contents of the file actually changed.

## Release Notes

Here is the full list of everything that changed in the CommandBox 3.4.0 release.

### Bug

* \[[COMMANDBOX-471](https://ortussolutions.atlassian.net/browse/COMMANDBOX-471)] - Adobe Servers won't start offline
* \[[COMMANDBOX-472](https://ortussolutions.atlassian.net/browse/COMMANDBOX-472)] - start serverConfigFile=myServer.json doesn't load json settings
* \[[COMMANDBOX-475](https://ortussolutions.atlassian.net/browse/COMMANDBOX-475)] - Adobe web.xml Flex config path is wrong after first engine start
* \[[COMMANDBOX-480](https://ortussolutions.atlassian.net/browse/COMMANDBOX-480)] - Error checking whether server is running
* \[[COMMANDBOX-484](https://ortussolutions.atlassian.net/browse/COMMANDBOX-484)] - cflib-coldbox endpoint creates invalid CFML for Adobe
* \[[COMMANDBOX-485](https://ortussolutions.atlassian.net/browse/COMMANDBOX-485)] - write history before command finishes
* \[[COMMANDBOX-491](https://ortussolutions.atlassian.net/browse/COMMANDBOX-491)] - Coldbox create interceptor doesn't create test with proper CFC mapping
* \[[COMMANDBOX-492](https://ortussolutions.atlassian.net/browse/COMMANDBOX-492)] - war path not stored in server.json as relative path
* \[[COMMANDBOX-494](https://ortussolutions.atlassian.net/browse/COMMANDBOX-494)] - CFML upgrades don't delete removed files
* \[[COMMANDBOX-496](https://ortussolutions.atlassian.net/browse/COMMANDBOX-496)] - Forgetting a named server deletes the 'default' server.json too

### New Feature

* \[[COMMANDBOX-476](https://ortussolutions.atlassian.net/browse/COMMANDBOX-476)] - allow bump Git tag to have custom prefix

### Improvement

* \[[COMMANDBOX-477](https://ortussolutions.atlassian.net/browse/COMMANDBOX-477)] - testbox create bdd include describe and it block
* \[[COMMANDBOX-481](https://ortussolutions.atlassian.net/browse/COMMANDBOX-481)] - Allow server list to filter partial server names
* \[[COMMANDBOX-486](https://ortussolutions.atlassian.net/browse/COMMANDBOX-486)] - Sort tab completion options
* \[[COMMANDBOX-487](https://ortussolutions.atlassian.net/browse/COMMANDBOX-487)] - Also negate boolean options with "no" in front
* \[[COMMANDBOX-488](https://ortussolutions.atlassian.net/browse/COMMANDBOX-488)] - Stop model scaffolding with empty names
* \[[COMMANDBOX-489](https://ortussolutions.atlassian.net/browse/COMMANDBOX-489)] - Handle Control-C better in the shell
* \[[COMMANDBOX-493](https://ortussolutions.atlassian.net/browse/COMMANDBOX-493)] - Improve check for previously-installed package
* \[[COMMANDBOX-495](https://ortussolutions.atlassian.net/browse/COMMANDBOX-495)] - Don't update modified date of server.json unless actually modified.
* \[[COMMANDBOX-497](https://ortussolutions.atlassian.net/browse/COMMANDBOX-497)] - Starting named server inherits same server.json settings
