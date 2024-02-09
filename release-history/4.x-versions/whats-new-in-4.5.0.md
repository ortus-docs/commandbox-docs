# What's New in 4.5.0

&#x20;The main features of CommandBox 4.5.0 are:

* Ability to install OpenJDK automatically for your servers to use ([read more](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/custom-java-version#automatic-java-download))
* Environment Variables in the shell ([read more](https://commandbox.ortusbooks.com/usage/environment-variables))
* Support for Forgebox Enterprise (TBA soon)
* JRE Bundled CommandBox installs now use OpenJDK instead of Oracle JDK
* TestBox Code Coverage integration ([read more](https://commandbox.ortusbooks.com/testbox-integration/test-runner#code-coverage))

I already wrote a fairly comprehensive overview of the new features and big fixes here.  Go read it:

[https://www.ortussolutions.com/blog/commandbox-450-rc-release-candidate-ready-for-testing](https://www.ortussolutions.com/blog/commandbox-450-rc-release-candidate-ready-for-testing)

Note, there are two backwards incompatible changes.  The first is we turned OFF directory browsing by default on servers.  You can easily get the old behavior back with

```bash
config set server.defaults.web.directoryBrowsing=true
```

The second is that unhandled errors in the shell no longer show the stack trace (you probably wouldn't have noticed if I didn't tell you!)  Get the old behavior back with:

```bash
config set verboseErrors=true
```

## Release Notes

Here's the full list of everything that went into this release.

### Bug

* \[[COMMANDBOX-784](https://ortussolutions.atlassian.net/browse/COMMANDBOX-784)] - editing in the shell prompt is buggy while using Gitbash in VSCode
* \[[COMMANDBOX-895](https://ortussolutions.atlassian.net/browse/COMMANDBOX-895)] - Passing positional args to task errors with required param
* \[[COMMANDBOX-897](https://ortussolutions.atlassian.net/browse/COMMANDBOX-897)] - Passing command string as single arg to box fails
* \[[COMMANDBOX-899](https://ortussolutions.atlassian.net/browse/COMMANDBOX-899)] - Exitting recipe with exit code errors
* \[[COMMANDBOX-901](https://ortussolutions.atlassian.net/browse/COMMANDBOX-901)] - external module mappings broken in CommandBox 4
* \[[COMMANDBOX-903](https://ortussolutions.atlassian.net/browse/COMMANDBOX-903)] - Incorrect behavior when parsing unmatched quotes
* \[[COMMANDBOX-915](https://ortussolutions.atlassian.net/browse/COMMANDBOX-915)] - Cruft left in temp folder
* \[[COMMANDBOX-917](https://ortussolutions.atlassian.net/browse/COMMANDBOX-917)] - Silence annoying ESAPI warnings
* \[[COMMANDBOX-919](https://ortussolutions.atlassian.net/browse/COMMANDBOX-919)] - Warnings on java 11 about illegal reflective access

### New Feature

* \[[COMMANDBOX-516](https://ortussolutions.atlassian.net/browse/COMMANDBOX-516)] - Add concept of env vars for commands to use
* \[[COMMANDBOX-906](https://ortussolutions.atlassian.net/browse/COMMANDBOX-906)] - Add preCommandParamProcess interception point
* \[[COMMANDBOX-907](https://ortussolutions.atlassian.net/browse/COMMANDBOX-907)] - outdated commands now verify packages in parallel
* \[[COMMANDBOX-908](https://ortussolutions.atlassian.net/browse/COMMANDBOX-908)] - Automatically download JRE for server if specified by version range
* \[[COMMANDBOX-910](https://ortussolutions.atlassian.net/browse/COMMANDBOX-910)] - Support multiple ForgeBox endpoints
* \[[COMMANDBOX-911](https://ortussolutions.atlassian.net/browse/COMMANDBOX-911)] - New Java endpoint that ties into the AdpotOpenJDK builds
* \[[COMMANDBOX-914](https://ortussolutions.atlassian.net/browse/COMMANDBOX-914)] - Make exit code of native binary from run command available in the exception that is thrown
* \[[COMMANDBOX-916](https://ortussolutions.atlassian.net/browse/COMMANDBOX-916)] - Pull Code Coverage data on "testbox run"
* \[[COMMANDBOX-921](https://ortussolutions.atlassian.net/browse/COMMANDBOX-921)] - Allow recipe args to be used as environment variables for that command

### Improvement

* \[[COMMANDBOX-896](https://ortussolutions.atlassian.net/browse/COMMANDBOX-896)] - Add ETA to progress bar when downloading
* \[[COMMANDBOX-898](https://ortussolutions.atlassian.net/browse/COMMANDBOX-898)] - Improve default handling of JVM heap size
* \[[COMMANDBOX-900](https://ortussolutions.atlassian.net/browse/COMMANDBOX-900)] - Default directoryBrowsing to false
* \[[COMMANDBOX-902](https://ortussolutions.atlassian.net/browse/COMMANDBOX-902)] - Allow box to be called with a single string containing a command chain
* \[[COMMANDBOX-904](https://ortussolutions.atlassian.net/browse/COMMANDBOX-904)] - Prevent folder endpoint from picking up folders in CWD on install
* \[[COMMANDBOX-909](https://ortussolutions.atlassian.net/browse/COMMANDBOX-909)] - Hide stack trace by default when CLI errors
* \[[COMMANDBOX-922](https://ortussolutions.atlassian.net/browse/COMMANDBOX-922)] - Allow recipe command to accept arbitrary commands directly
* \[[COMMANDBOX-923](https://ortussolutions.atlassian.net/browse/COMMANDBOX-923)] - Include mapping-tag in rewrite exclusion list
* \[[COMMANDBOX-924](https://ortussolutions.atlassian.net/browse/COMMANDBOX-924)] - Update JRE builds to use OpenJDK instead of Oracle JDK
* \[[COMMANDBOX-925](https://ortussolutions.atlassian.net/browse/COMMANDBOX-925)] - Provide all other args to command completor UDFs
* \[[COMMANDBOX-926](https://ortussolutions.atlassian.net/browse/COMMANDBOX-926)] - Announce postInstall interceptions even if package was found to be already installed
