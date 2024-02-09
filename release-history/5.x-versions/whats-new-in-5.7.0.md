# What's New in 5.7.0

### Check Out The New Stuff

The notable updates include:

* Updated version of JBoss Undertow, which contains security fixes
* Updated version of Lucee to 5.3.10 which also contains library security updates
* New "artifacts prune" command to remove older artifacts that haven't been used recently
* Improved "upgrade" command which can also update new jars (you'll be able to use this in the NEXT update!)
* Support for PFX cert file for server SSL

And here are the full release notes:

### New Feature

#### [COMMANDBOX-1535](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1535) update coldbox resources to support api resources

#### [COMMANDBOX-1536](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1536) artifacts prune command

### Bug

#### [COMMANDBOX-1507](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1507) Use user.home instead of user.dir to test case sensitivity

#### [COMMANDBOX-1510](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1510) Cursor position get's off-track when using a multiselect with content that exceeds the terminal size

#### [COMMANDBOX-1511](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1511) ModCFML doesn't save actual Lucee webConfigDir

#### [COMMANDBOX-1513](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1513) Runwar server's regex path info filter doesn't take servlet context into account

#### [COMMANDBOX-1517](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1517) Two servers get same name when moving folder

#### [COMMANDBOX-1518](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1518) Can't pass Windows drive letter to "open" command

#### [COMMANDBOX-1519](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1519) Error passing empty version to "java search" command

#### [COMMANDBOX-1524](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1524) Interactive job logs don't obey terminal width config setting

#### [COMMANDBOX-1525](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1525) REPL errors when representing some Java objects

#### [COMMANDBOX-1527](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1527) recipe file validation fails

#### [COMMANDBOX-1528](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1528) Java integration doesn't recognize ARM CPUs

#### [COMMANDBOX-1533](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1533) coldbox resources command failing on relative models directory not found

### Improvement

#### [COMMANDBOX-1378](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1378) make "upgrade" command handle new jars

#### [COMMANDBOX-1499](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1499) Support pfx format for server certs

#### [COMMANDBOX-1512](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1512) Have fileAppend (>>) and fileWrite (>) commands create missing parent directories

#### [COMMANDBOX-1514](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1514) REPL obey verboseErrors config setting

#### [COMMANDBOX-1516](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1516) cfpm doesn't find Adobe server with different web root

#### [COMMANDBOX-1520](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1520) Make client cert CGI vars available immediately after SSL renegotiation

#### [COMMANDBOX-1521](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1521) Add --json to artifacts list

#### [COMMANDBOX-1523](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1523) Touch artifacts on use so we can prune unused ones easier

#### [COMMANDBOX-1534](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1534) Update coldbox resources command to latest version

### Task

#### [COMMANDBOX-1526](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1526) Update bundled JRE to 11.0.17+8

#### [COMMANDBOX-1529](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1529) Update Undertow lib in Runwar

#### [COMMANDBOX-1530](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1530) Update json-smart-mini and Lucee libs
