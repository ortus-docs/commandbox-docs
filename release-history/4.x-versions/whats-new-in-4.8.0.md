# What's new in 4.8.0

###  Cached HTTP Downloads

You can now cache downloads using the HTTP\(S\) endpoints using the following syntaxes:

```bash
install https+cached://downloads.ortussolutions.com/ortussolutions/coldbox-modules/cbi18n/1.4.0/cbi18n-1.4.0.zip

start cfengine=http+cached://update.lucee.org/rest/update/provider/forgebox/5.3.3.60-RC
```

This will speed up builds.

### Change to Previous Directory

Thanks to a pull request from John Berquist, we've borrowed a Bash and Powershell feature of being able to change back to your previous working directory by typing this:

```bash
cd -
```

### Better Tab Completion

Thanks to more pull requests from John Berquist, you can use file and folder based tab completion when typing native binaries from CommandBox 

```bash
!foo bar "C:/Program Files/baz/myFile.cf_
```

And tab completion also works better now when typing a quoted string such as a file path that contains a space.  This is a huge timesaver!

```bash
cd "C:/program Fi_
```

### Access to Intercept data in package scripts

Package scripts that are fired from internal interception points, can access any intercept data via environment variables.  This example writes a file into a server home directory when the server starts, using an environment variable to dynamically find the correct path.

```bash
package set scripts.onServerStart="touch \$ {interceptData.SERVERINFO.serverHomeDirectory}/hi.txt"
```

## Release Notes

Here are the full release notes for CommandBox 4.8.0:

### Bug

* \[[COMMANDBOX-991](https://ortussolutions.atlassian.net/browse/COMMANDBOX-991)\] - Can't always install modules - git error: Directory already exists
* \[[COMMANDBOX-994](https://ortussolutions.atlassian.net/browse/COMMANDBOX-994)\] - regex metachars not escaped properly in first token of run command
* \[[COMMANDBOX-998](https://ortussolutions.atlassian.net/browse/COMMANDBOX-998)\] - testbox watch command doesn't obey verbose flag in box.json
* \[[COMMANDBOX-999](https://ortussolutions.atlassian.net/browse/COMMANDBOX-999)\] - Sometimes line breaks leak to console when using expansions
* \[[COMMANDBOX-1000](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1000)\] - Pass ad-hoc parameters to package scripts
* \[[COMMANDBOX-1003](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1003)\] - Servers bound to 0.0.0.0 don't open useful browser URL
* \[[COMMANDBOX-1030](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1030)\] - unicode chars not read from readme files when publishing
* \[[COMMANDBOX-1040](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1040)\] - Native OS execution doesn't handle exit on fail for \*nix
* \[[COMMANDBOX-1041](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1041)\] - Install path not respected when createPackageDirectory set to false

### New Feature

* \[[COMMANDBOX-1002](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1002)\] - Add cached version of HTTP\(S\) endpoints
* \[[COMMANDBOX-1031](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1031)\] - Output binary objects in REPL
* \[[COMMANDBOX-1039](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1039)\] - Add support for "cd -"

### Improvement

* \[[COMMANDBOX-993](https://ortussolutions.atlassian.net/browse/COMMANDBOX-993)\] - Announce onServerStop when stopping a --console server
* \[[COMMANDBOX-996](https://ortussolutions.atlassian.net/browse/COMMANDBOX-996)\] - Support path completion with "run" or "!" command
* \[[COMMANDBOX-997](https://ortussolutions.atlassian.net/browse/COMMANDBOX-997)\] - Expand log output of failed job steps
* \[[COMMANDBOX-1001](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1001)\] - Pass interceptData to package scripts
* \[[COMMANDBOX-1004](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1004)\] - Allow tab completion on quoted parameters
* \[[COMMANDBOX-1033](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1033)\] - Editor on Linux
* \[[COMMANDBOX-1042](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1042)\] - Return actual exit code of server process from server start
* \[[COMMANDBOX-1045](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1045)\] - Java install endpoint allows invalid slugs
* \[[COMMANDBOX-1046](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1046)\] - Pass CommandBox shell env vars to server starts
* \[[COMMANDBOX-1047](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1047)\] - Better detection of CF Engine when using HTTP provider

