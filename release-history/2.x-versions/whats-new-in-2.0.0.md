# What's New in 2.0.0

### We Love Lucee

The biggest feature is the switch-over from Railo to Lucee as the underlying CLI engine that powers the REPL and commands.  The embedded server now also runs Lucee 4.5 as well.  If you require a Railo embedded server, you will need to stay on CommandBox 1.1.1 for now. &#x20;

### Endpoint Support

Another major new feature is support for different endpoints when installing packages in addition to ForgeBox.  Now packages can be installed from the following locations:

* Local zip file
* Local folder
* HTTP/HTTPS URL that points to a package zip
* ForgeBox (default)

The ForgeBox endpoint now also has rudimentary support for targeting a specific version.  If you request a specific version of a package to be installed, and it is in your artifacts cache, no network calls will be made.  This allows completely offline installations! Here are some examples:

```bash
install coldbox
install coldbox@4.0.0
install C:/myZippedPackages/foobar.zip
install C:/myUnzippedPackages/foobar/
install http://site.com/foobar.zip
install https://site.com/foobar.zip
```

&#x20;We also have a nice collection of bug fixes.  Below are the full release notes for CommandBox 2.0.0.

### **Release Notes - CommandBox - Version 2.0.0**

### Bug

* \[[COMMANDBOX-211](https://ortussolutions.atlassian.net/browse/COMMANDBOX-211)] - CommandBox caches .cfm files between executions
* \[[COMMANDBOX-214](https://ortussolutions.atlassian.net/browse/COMMANDBOX-214)] - Lucee version leaves tons of old jars on upgrade
* \[[COMMANDBOX-218](https://ortussolutions.atlassian.net/browse/COMMANDBOX-218)] - Script repl confused on paranthesis or quotes
* \[[COMMANDBOX-223](https://ortussolutions.atlassian.net/browse/COMMANDBOX-223)] - coldbox create model command doesn't escape "open" parameters
* \[[COMMANDBOX-224](https://ortussolutions.atlassian.net/browse/COMMANDBOX-224)] - Coldbox create model creates incorrect testcase w/ no methods
* \[[COMMANDBOX-225](https://ortussolutions.atlassian.net/browse/COMMANDBOX-225)] - Calling \`forgebox slugcheck\` with empty slugname throws error
* \[[COMMANDBOX-232](https://ortussolutions.atlassian.net/browse/COMMANDBOX-232)] - osx brew installation broken for commandbox 2.0
* \[[COMMANDBOX-237](https://ortussolutions.atlassian.net/browse/COMMANDBOX-237)] - Application times out and wirebox references die
* \[[COMMANDBOX-238](https://ortussolutions.atlassian.net/browse/COMMANDBOX-238)] - Dev installation w/out package directory overwrites box.json

### New Feature

* \[[COMMANDBOX-159](https://ortussolutions.atlassian.net/browse/COMMANDBOX-159)] - Switch CommandBox core to Lucee
* \[[COMMANDBOX-215](https://ortussolutions.atlassian.net/browse/COMMANDBOX-215)] - Multi Endpoint support
