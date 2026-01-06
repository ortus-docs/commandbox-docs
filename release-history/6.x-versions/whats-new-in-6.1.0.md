---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/Bw6M3PI3e5HgLVcKZz0G/release-history/6.x-versions/whats-new-in-6.1.0
---

# What's New in 6.1.0

Here's an overview of the notable changes in this release.

### Install Lucee core/lex files

The install command will automatically find a local Lucee Server when installing a package that is a Lex file.  There is also now a command called **server lucee-deploy** to help with installing Lex files (Lucee Extensions) and LCO files (Lucee Core).

It will accept an absolute or relative local file path:

```
server lucee-deploy myFile.lex
```

or an HTTP URL to download

```
server lucee-deploy https://domain.com/path/to/Lucee-core-patch.lco
```

You can also override the name of the CommandBox server to install into, regardless of what working directory the shell is in:

```
server lucee-deploy myFile.lex myServer
```

The command will copy the lex or lco file to the deploy folder of the chosen Lucee server

More info here: [https://commandbox.ortusbooks.com/embedded-server/install-lucee-core-lex-files](https://commandbox.ortusbooks.com/embedded-server/install-lucee-core-lex-files)

### Package Management binary hash verification

CommandBox now has the capability to verify the MD5 hash of a binary upon installation.  When publishing packages to ForgeBox using ForgeBox's built-in S3 storage, the binary hash will be auto-calculated and stored in ForgeBox along with the package details and automatically checked on every future download. &#x20;

More info here: [https://commandbox.ortusbooks.com/package-management/creating-packages#custom-binary-hash](https://commandbox.ortusbooks.com/package-management/creating-packages#custom-binary-hash)

### WebSocket Support

CommandBox supports WebSockets natively via our [SocketBox library](https://forgebox.io/view/socketbox).  The WebSocket server in CommandBox is not really a separate "server" per se, since it’s on the same port. It’s just an upgrade listener which will upgrade any WS requests.

This websocket integration will work for **Lucee**, **Adobe**, and **BoxLang** alike as it passes incoming messages to the app via an "internal" HTTP request to **/WebSocket.cfc?method=onProcess** where the CF/BL code can handle it. The incoming request will have all cookies, headers, hostname, etc that the original websocket connection was started with, so normal CGI variables and session scopes should work fine.

You need to create a custom **/WebSocket.cfc** class should extend one of the classes described below in this library which provides the base functionality.

More info here: [https://commandbox.ortusbooks.com/embedded-server/websocket-support](https://commandbox.ortusbooks.com/embedded-server/websocket-support)

And here: [https://forgebox.io/view/socketbox](https://forgebox.io/view/socketbox)

### Library Updates

We've bumped the versions of several internal libraries including&#x20;

* Lucee (**5.4.6.9**)
* JBoss Undertow (**2.2.37.Final**)
* Bundled JRE (**11.0.25+9**)

### Release Notes

Here is the full list of tickets closed in the 6.1.0 release.

#### New Feature

[COMMANDBOX-1630](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1630) Updates to run BoxLang

[COMMANDBOX-1635](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1635) Add command to deploy Lucee lex or lco files

[COMMANDBOX-1637](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1637) Check if an entry has a hash associated to it and validate it

[COMMANDBOX-1638](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1638) Create a hash of the binary when storing the zip and return it to ForgeBox

[COMMANDBOX-1642](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1642) WebSocket Server

#### Improvement

[COMMANDBOX-1620](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1620) Sort by date last started when finding a server by web root

[COMMANDBOX-1622](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1622) Make semantic version prerelease identifiers not case sensitive

[COMMANDBOX-1627](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1627) default servlet pass predicate include Boxlang files

#### Bug

[COMMANDBOX-1616](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1616) Legacy SSL config doesn't respect enable = false

[COMMANDBOX-1617](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1617) Static web server no longer works

[COMMANDBOX-1619](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1619) second level dependencies inside of a top level dependency's box.json always install latest stable version

[COMMANDBOX-1621](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1621) "Java search" returns no results

[COMMANDBOX-1623](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1623) framework rewrites double-encode path info

[COMMANDBOX-1625](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1625) CommandBox 6 not putting trailing slashes in URL

[COMMANDBOX-1629](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1629) SES paths return false from is-file predicate for value files with a path info

[COMMANDBOX-1633](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1633) backslash in JSON object keys not escaped when printing JSON

[COMMANDBOX-1643](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1643) forgebox version-debug errors

#### Task

[COMMANDBOX-1626](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1626) Update to Undertow 2.2.33.Final

[COMMANDBOX-1631](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1631) Update to Lucee 5.4.6.9

[COMMANDBOX-1632](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1632) Update bundled JRE to 11.0.23+9
