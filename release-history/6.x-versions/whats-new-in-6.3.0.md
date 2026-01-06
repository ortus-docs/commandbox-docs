# What's New in 6.3.0

Here's an overview of the new goodies in this release.

### box-lock.json Package Lock File

This release introduces lock file support for CommandBox package management, ensuring consistent dependency versions across installations. When you use the new --lock flag with the install command, CommandBox creates a box-lock.json file that pins exact versions of your dependencies. The system intelligently maintains these locked versions during install, uninstall, and update operations while respecting your semver ranges—automatically updating the lock file when your requested version falls outside the locked range or when using force installs. This feature works seamlessly alongside existing workflows and has no impact on projects that don't use lock files.

```java
install --lock
```

### package link BoxLang modules

When developing on BoxLang modules, you can now use the **package link** command to symlink those modules to your BoxLang home's modules folder.

```java
package link ~/.boxlang/modules
```

### Override Module settings with env vars

This didn't work in the past due to chicken/egg issues and the order things loaded.  Now you can use global env vars to override how modules behave, which is nice for CI environments.

```java
box_config_modules_moduleName_settingName=value
```

### STOMP/Websocket heartbeat improvement

After some real-world testing with our STOMP/Websocket implementation, we've enhanced STOMP heartbeats to be replied to directly from Undertow/Java without hitting your CF code. This improves performance and removes clutter in your performance profiling tools.

### Server Process Working Directory

When starting a server, the Java server process will be the directory that the server.json lives in, otherwise the web root.  This affects how Java's default behavior of expanding relative paths works, and allows you to use the user.dir java system property in your config files for relative paths.

### BoxLang-aware rewrites

The default rewrite rule will automatically use index.bxm instead of index.cfm when you start a BoxLang server which has an index.bxm file in the web root.  You can now also override the front controller with a custom server rule.

```java
framework-rewrite( 'myFrontController.bxm' )
```

### Updated Dependencies

We've also updated to the latest Undertow and Lucee libraries.  Note this Undertow update includes some security fixes from RedHat.

## Release Notes ⚡️

Here are the full release notes for this release.

#### Bug

[COMMANDBOX-1666](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1666) server hangs on start if port was in use

[COMMANDBOX-1668](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1668) terminal doesn't obey non interactive mode when asking for missing command args

[COMMANDBOX-1670](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1670) don't call exit from shutdown hook

[COMMANDBOX-1676](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1676) web.xml parsing not properly taking XML schema into account

[COMMANDBOX-1680](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1680) custom error pages to static files don't work on non-get/post requests

#### Improvement

[COMMANDBOX-1661](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1661) Allow package link to work for boxlang-modules

[COMMANDBOX-1669](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1669) Allow global env file to override module settings

[COMMANDBOX-1671](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1671) Freshen up random quotes that appear on startup

[COMMANDBOX-1673](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1673) return STOMP heartbeat responses from runwar

[COMMANDBOX-1674](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1674) Default rewrite file to bxm for boxlang apps, allow configuration of custom file

[COMMANDBOX-1675](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1675) Set working dir of server process to the project

[COMMANDBOX-1677](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1677) Don't write CF or BL extensions with default rewrite rule

#### New Feature

[COMMANDBOX-1315](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1315) box-lock.json



reCAPTCHA
