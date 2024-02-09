# What's New in 5.1.1

This release was primarily to address a regression in 5.1.0 affecting Mac OS users who tried to start Lucee servers.  If you see an error similar to this on a Lucee server and you're running a Mac and CommandBox 5.1.0, then this release will fix it for you.

```
lucee.runtime.exp.NativeException: mac os x is not a supported OS platform.
```

If you are upgrading from CommandBox 5.1.0, there are only a handful of tickets which are listed below.  If you are updating from an earlier version of CommandBox, please check out our [5.0.0](https://www.ortussolutions.com/blog/commandbox-500-released) and [5.1.0](https://www.ortussolutions.com/blog/commandbox-510-released) release blogs.

## Release notes

### Bug

* \[[COMMANDBOX-1107](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1107)] - Overzealous gitignore matching of parent directories when zipping up for ForgeBox storage
* \[[COMMANDBOX-1173](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1173)] - Enabling SSL results in some CFHTTP requests to fail.
* \[[COMMANDBOX-1178](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1178)] - writedump failing in Lucee
* \[[COMMANDBOX-1179](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1179)] - File globbing matching partial file names

### Improvement

* \[[COMMANDBOX-1181](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1181)] - Allow for verbose startup without debug logging of requests
