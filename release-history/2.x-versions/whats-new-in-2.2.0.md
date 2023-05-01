# What's New in 2.2.0

## Release Notes

### Bug

* \[[COMMANDBOX-104](https://ortussolutions.atlassian.net/browse/COMMANDBOX-104)] - Execute command doesn't work in interactive shell
* \[[COMMANDBOX-112](https://ortussolutions.atlassian.net/browse/COMMANDBOX-112)] - Testbox create commands break if testname includes package
* \[[COMMANDBOX-270](https://ortussolutions.atlassian.net/browse/COMMANDBOX-270)] - installpaths not added when not creating package directory
* \[[COMMANDBOX-271](https://ortussolutions.atlassian.net/browse/COMMANDBOX-271)] - Git endpoint psses java.io.File instead of string
* \[[COMMANDBOX-273](https://ortussolutions.atlassian.net/browse/COMMANDBOX-273)] - HTTP Endpoint package name guessing doesn't account for periods in file name
* \[[COMMANDBOX-274](https://ortussolutions.atlassian.net/browse/COMMANDBOX-274)] - When you do an 'update' command, it updates the modules but does not update the box.json with the latest version
* \[[COMMANDBOX-275](https://ortussolutions.atlassian.net/browse/COMMANDBOX-275)] - SQL Server JDBC driver doesn't work
* \[[COMMANDBOX-282](https://ortussolutions.atlassian.net/browse/COMMANDBOX-282)] - Sign Debian packages

### New Feature

* \[[COMMANDBOX-258](https://ortussolutions.atlassian.net/browse/COMMANDBOX-258)] - Update the status command to output the results in json
* \[[COMMANDBOX-268](https://ortussolutions.atlassian.net/browse/COMMANDBOX-268)] - Update coldbox model generator to allow the creation of accessors
* \[[COMMANDBOX-269](https://ortussolutions.atlassian.net/browse/COMMANDBOX-269)] - Add ability to generate properties on coldbox model generations
* \[[COMMANDBOX-272](https://ortussolutions.atlassian.net/browse/COMMANDBOX-272)] - Update all BDD tests to fail by default to promote refactoring and process
* \[[COMMANDBOX-277](https://ortussolutions.atlassian.net/browse/COMMANDBOX-277)] - CFLib endpoint
* \[[COMMANDBOX-278](https://ortussolutions.atlassian.net/browse/COMMANDBOX-278)] - RIAForge Endpoint

### Improvement

* \[[COMMANDBOX-204](https://ortussolutions.atlassian.net/browse/COMMANDBOX-204)] - Add a directory argument for the REPL
* \[[COMMANDBOX-265](https://ortussolutions.atlassian.net/browse/COMMANDBOX-265)] - REPL's handling of functions that output content
* \[[COMMANDBOX-267](https://ortussolutions.atlassian.net/browse/COMMANDBOX-267)] - Optimize JVM Memory Arguments to Prevent PermGen and Java.lang.outOfMemory errors
* \[[COMMANDBOX-276](https://ortussolutions.atlassian.net/browse/COMMANDBOX-276)] - Improve error message when ForgeBox REST API is down
* \[[COMMANDBOX-280](https://ortussolutions.atlassian.net/browse/COMMANDBOX-280)] - Update to latest Lucee stable build
* \[[COMMANDBOX-281](https://ortussolutions.atlassian.net/browse/COMMANDBOX-281)] - Update to latest ColdBox application templates
