# Server Versions

### Remember to use Semantic Versions

One minor difference to keep in mind is Lucee server and Adobe ColdFusion use a typical versioning scheme for java projects which looks like this:

```bash
<major>.<minor>.<patch>.<build>[-<preReleaseID>]

# Ex of an unstable build
5.3.4.84-SNAPSHOT

# Or a stable build
5.3.4.80
```

However, [CommandBox](https://commandbox.ortusbooks.com/) and [ForgeBox](https://www.forgebox.io/) use the [npm-flavored semantic versioning](https://commandbox.ortusbooks.com/package-management/semantic-versioning) \(semver\) which is slightly different.  Basically the build ID is moved to the end after a plus \(+\) sign.

```bash
<major>.<minor>.<patch>[-<preReleaseID>]+<build>

# Ex of an unstable build
5.3.4-SNAPSHOT+84

# Or a stable build
5.3.4+80
```

The important thing to remember is, when starting a server via CommandBox **always use the second format shown above** since that is how ForgeBox recognizes each release. 

{% hint style="info" %}
As of version `5.3.0`, CommandBox will also recognize the fourth digit in `1.2.3.4` as a build ID if there is no plus sign in the version.  This makes `5.3.4+80` and `5.3.4.80` equivalent.
{% endhint %}

### What versions exist?

Questions about what versions are available?  No problem!  Here are some ways you can find out:

* Visit the ForgeBox listing for the Lucee package and view all the versions in the versions tab  [https://www.forgebox.io/view/lucee\#versions](https://www.forgebox.io/view/lucee#versions) [https://www.forgebox.io/view/adobe\#versions](https://www.forgebox.io/view/adobe#versions)
* View the last few versions via the CLI with the command `forgebox show lucee`
* Start typing your `cfengine` and hit the &lt;tab&gt; key to invoke the tab-completion feature.  This actually phones out to ForgeBox as you hit tab to get the current valid list of versions that match what you've typed so far

You can also follow the Lucee bleeding edge, which means every time you start your CommandBox server you'll get the very latest Lucee snapshot release.  Please only use this for local development and not production!

```text
server start cfengine=lucee@be
```

## Pinning Exact Version

It is a nice feature of CommandBox to have it automatically grab the latest version of your favorite CF engine every time it starts.

```bash
# Latest Lucee 5.3.7 build
server start cfengine=lucee@5.3.7

# Latest Adobe 2018 update
server start cfengine=adobe@2018

# Lucee bleeding edge-- latest snapshot
server start cfengine=lucee@be
```

However, you may have good reason to NEVER want a new version automatically installed.  In order to do this, you must specify a COMPLETE version number, including the build number, making sure to use the proper version format.  This means you need a **major**, **minor**, **patch**, and **build** number.

```bash
# Pinned exact Adobe version
server start cfengine=adobe@2018.0.10+320417

# Pinned exact Lucee version
server start cfengine=lucee@5.3.7+48
```

## **Lucee Light builds on ForgeBox**

Lucee has a modular core and comes bundled with a bunch of extensions that approximate the functionality that comes bundled with Adobe ColdFusion.  But that means you are loading the Hibernate libraries, PDF libraries or JDBC drivers even if you don't need them. There is a second type of Lucee server called "Lucee Light" which contains all the core engine, but with zero extensions.  People creating custom docker builds for example will start with Lucee Light and then add back only the extensions their app needs.  To get a feel for all the Lucee extensions available, see [this page](https://download.lucee.org/) that lists all official extensions.

We're now publishing CF Engines to ForgeBox based on the Lucee Light builds which allows you to start up a Lucee Light server. These are under a ForgeBox package named `lucee-light` and we've also backfilled all the same versions that exist for the normal `lucee` engine.  Note, the three bullets points above apply to Lucee Light as well,  Just replace `lucee` with `lucee-light` and you're good to go.

```bash
# Latest stable
server start cfengine=lucee-light

# Specific version
server start cfengine=lucee-light@5.3.4.77

# Bleeding edge
server start cfengine=lucee-light@be
```

If you have questions about how to install extensions into a Lucee light server, please hit us up on Slack and we can show you several ways to manage that.



