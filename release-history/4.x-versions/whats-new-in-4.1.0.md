# What's New in 4.1.0

## Release Highlights

* Fixed the annoying "server spanner" error that \*nix users saw when starting servers.
* Updated CLI engine (and default server) version to **Lucee 5.2.7.63** which includes an important security fix.
* Added a new "noninteractive" setting to improve the output on build servers like Jenkins or Travis-CI
* Task runner metadata changes picked up without needing to reload the CLI
* Ability to load ad-hoc modules on the fly from Task Runners

## Release Notes

Here's the full list of tickets we addressed in the 4.1.0 release.  Thanks to those who send me pull requests for some of these fixes and features!

### Bug

* \[[COMMANDBOX-791](https://ortussolutions.atlassian.net/browse/COMMANDBOX-791)] - server start on linux: "/bin/bash not found" message displays
* \[[COMMANDBOX-799](https://ortussolutions.atlassian.net/browse/COMMANDBOX-799)] - Improve port binding logic
* \[[COMMANDBOX-802](https://ortussolutions.atlassian.net/browse/COMMANDBOX-802)] - Task component metadata is not refreshed before each run
* \[[COMMANDBOX-806](https://ortussolutions.atlassian.net/browse/COMMANDBOX-806)] - Error when invalid UTF-8 characters are in servers.json

### New Feature

* \[[COMMANDBOX-669](https://ortussolutions.atlassian.net/browse/COMMANDBOX-669)] - Localized CommandBox Modules
* \[[COMMANDBOX-793](https://ortussolutions.atlassian.net/browse/COMMANDBOX-793)] - Add new slugify function to the formatter utility
* \[[COMMANDBOX-794](https://ortussolutions.atlassian.net/browse/COMMANDBOX-794)] - When creating coldbox skeleton apps, clear out the basic name, slug, version, location and scripts so the user can configure them
* \[[COMMANDBOX-797](https://ortussolutions.atlassian.net/browse/COMMANDBOX-797)] - Added new bundles,labels,verbose,directory arguments to the testbox watch command to allow for granular executions
* \[[COMMANDBOX-798](https://ortussolutions.atlassian.net/browse/COMMANDBOX-798)] - Added verbose options to passthrough to the testbox runner so it true it can return the debug buffer

### Improvement

* \[[COMMANDBOX-792](https://ortussolutions.atlassian.net/browse/COMMANDBOX-792)] - Remove legacy installColdbox, installColdboxBE, installTestbox arguments from create commands
* \[[COMMANDBOX-796](https://ortussolutions.atlassian.net/browse/COMMANDBOX-796)] - Added ability to visualize the exception stacktraces with a configurable depth.
* \[[COMMANDBOX-800](https://ortussolutions.atlassian.net/browse/COMMANDBOX-800)] - Add setting to force "non interactive" shell that disables fancypants progress output
* \[[COMMANDBOX-803](https://ortussolutions.atlassian.net/browse/COMMANDBOX-803)] - Change user agent on downloader to get around proxies like cloudflare
* \[[COMMANDBOX-804](https://ortussolutions.atlassian.net/browse/COMMANDBOX-804)] - Store less "result" text in CommandBox's servers.json
* \[[COMMANDBOX-807](https://ortussolutions.atlassian.net/browse/COMMANDBOX-807)] - Upgrade core Lucee engine to 5.2.7.63
