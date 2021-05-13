# What's New in 4.6.0

## Release Notes

### Bug

* \[[COMMANDBOX-934](https://ortussolutions.atlassian.net/browse/COMMANDBOX-934)\] - Server commands can have huge delay on Windows
* \[[COMMANDBOX-937](https://ortussolutions.atlassian.net/browse/COMMANDBOX-937)\] - List artifacts alphabetically.
* \[[COMMANDBOX-939](https://ortussolutions.atlassian.net/browse/COMMANDBOX-939)\] - /usr/bin/open on Linux
* \[[COMMANDBOX-942](https://ortussolutions.atlassian.net/browse/COMMANDBOX-942)\] - Errors in command CFCs can cause box to exit completely during tab complete
* \[[COMMANDBOX-949](https://ortussolutions.atlassian.net/browse/COMMANDBOX-949)\] - Running native binary that returns lots of text can perform poorly
* \[[COMMANDBOX-950](https://ortussolutions.atlassian.net/browse/COMMANDBOX-950)\] - Interceptor service blows up if you register a module with an interceptor not matching any current states
* \[[COMMANDBOX-951](https://ortussolutions.atlassian.net/browse/COMMANDBOX-951)\] - Allow modules to register an interceptor with no currently valid states
* \[[COMMANDBOX-953](https://ortussolutions.atlassian.net/browse/COMMANDBOX-953)\] - Catch errors from desktop.isDesktopSupported\(\)

### New Feature

* \[[COMMANDBOX-930](https://ortussolutions.atlassian.net/browse/COMMANDBOX-930)\] - Allow system setting \(env var\) expansions in REPL
* \[[COMMANDBOX-932](https://ortussolutions.atlassian.net/browse/COMMANDBOX-932)\] - Improve task DSL to allow access to exit code
* \[[COMMANDBOX-944](https://ortussolutions.atlassian.net/browse/COMMANDBOX-944)\] - Add config setting to debug raw native command being used in the "run" command

### Improvement

* \[[COMMANDBOX-249](https://ortussolutions.atlassian.net/browse/COMMANDBOX-249)\] - Enforce correct casing conventions on scaffolding commands
* \[[COMMANDBOX-927](https://ortussolutions.atlassian.net/browse/COMMANDBOX-927)\] - Update propertyFile core module
* \[[COMMANDBOX-928](https://ortussolutions.atlassian.net/browse/COMMANDBOX-928)\] - Improve default ignores in box.json from init command.
* \[[COMMANDBOX-929](https://ortussolutions.atlassian.net/browse/COMMANDBOX-929)\] - Disable ping to time server host by default
* \[[COMMANDBOX-931](https://ortussolutions.atlassian.net/browse/COMMANDBOX-931)\] - Allow exit code to be returned via "return" keyword
* \[[COMMANDBOX-935](https://ortussolutions.atlassian.net/browse/COMMANDBOX-935)\] - Improve syntax highlighting in REPL
* \[[COMMANDBOX-938](https://ortussolutions.atlassian.net/browse/COMMANDBOX-938)\] - Checking interrupted status from inside a thread doesn't end the task/command
* \[[COMMANDBOX-943](https://ortussolutions.atlassian.net/browse/COMMANDBOX-943)\] - Remove hint from default CFC in Lucee for CommandBox CFCs with no hint of their own
* \[[COMMANDBOX-945](https://ortussolutions.atlassian.net/browse/COMMANDBOX-945)\] - Endpoint URL shows incorrectly for forgebox endpoints
* \[[COMMANDBOX-948](https://ortussolutions.atlassian.net/browse/COMMANDBOX-948)\] - Enhance tab complete for private slugs

