# What's New in 3.2.0

## Offline Server Starts

One of the biggest pieces of feedback we got from the CommandBox 3.1 release was that it requires an Internet connection to start a server. Since you can specify the version of the server you want to start, including semver ranges like 5.x, this required a trip to ForgeBox to check and see what the best version match was since it might have changed since the last time you started the server.

```bash
CommandBox> start cfengine=lucee@5.x
```

Now, if you provide an exact CF engine version number \(meaning you give us a major, minor, AND patch version\) CommandBox will skip phoning home to ForgeBox and will just continue with that version.

```bash
CommandBox> start cfengine=lucee@5.0.0
```

Remember that the version "5" de-sugars to "5.x.x" so you need to have all three numbers even if they're "0"

## Failsafe Server Starts <a id="failsafe-server-starts"></a>

We even went a step further. Sometimes ForgeBox may be down for maintenance or due to an outage and it was preventing people from being able to start their servers. If ForgeBox can't be reached for some reason while starting the server, we'll try again by comparing the version range you provided with the CF Engines already cached in your local artifacts cache. If we can find a version that satisfies what you asked for, we'll use it to start the server. That means you might not be getting the latest version of the engine from ForgeBox, but at least your server will start with that is has downloaded already.

## Unpublish Packages <a id="unpublish-packages"></a>

Unpublishing a package should be something you rarely need to do since once a package is published, someone else may be depending it for their app to run. However, we now have an unpublish command you can run from the CLI to remove a specific version of a package, or the entire package itself from ForgeBox.

```bash
CommandBox> unpublish
CommandBox> unpublish 1.2.3
```

## New Server Updates <a id="new-server-updates"></a>

We've also included the latest versions of Adobe ColdFusion 10, 11, and 2016 on ForgeBox. This is something we can update separately from our CommandBox releases, but they came at the same time so I bundled the announcements together :\) Here are the latest Adobe versions available:

* Adobe CF 10.0.20+299202
* Adobe CF 11.0.09+299201
* Adobe CF 2016.0.02+299200

## Bug Fixes <a id="bug-fixes"></a>

We also fixed a few bugs too. For example, targeting a Git tag stopped working in version 3.1 due to a library update, plus CommandBox's proxy settings weren't being used for all HTTP requests.

## Release Notes <a id="release-notes"></a>

Here's the full list of everything in version 3.2.0 of CommandBox. Click on the ticket numbers for more details in JIRA.

### Bugs <a id="bugs"></a>

* \[[COMMANDBOX-400](https://ortussolutions.atlassian.net/browse/COMMANDBOX-400)\] - Box install throws exception on git endpoint with commit-ish syntax
* \[[COMMANDBOX-406](https://ortussolutions.atlassian.net/browse/COMMANDBOX-406)\] - Starting server from diff directory by name uses wrong web root
* \[[COMMANDBOX-407](https://ortussolutions.atlassian.net/browse/COMMANDBOX-407)\] - semver isExactVersion returns true for 3 and 3.4
* \[[COMMANDBOX-408](https://ortussolutions.atlassian.net/browse/COMMANDBOX-408)\] - Proxy server not used in ForgeBox calls

### New Features <a id="new-features"></a>

* \[[COMMANDBOX-397](https://ortussolutions.atlassian.net/browse/COMMANDBOX-397)\] - Unpublish command
* \[[COMMANDBOX-402](https://ortussolutions.atlassian.net/browse/COMMANDBOX-402)\] - Show package URL location after publish, some consoles allow you to click and visit
* \[[COMMANDBOX-403](https://ortussolutions.atlassian.net/browse/COMMANDBOX-403)\] - Show number of packages in forgebox types
* \[[COMMANDBOX-404](https://ortussolutions.atlassian.net/browse/COMMANDBOX-404)\] - Add ForgeBox URL to show command as some consoles allow you to visit
* \[[COMMANDBOX-405](https://ortussolutions.atlassian.net/browse/COMMANDBOX-405)\] - Add ForgeBox URL to search command so consoles can click and open

### Tasks <a id="tasks"></a>

* \[[COMMANDBOX-409](https://ortussolutions.atlassian.net/browse/COMMANDBOX-409)\] - Add new patches for Adobe CF 10, 11, and 2016 to forgebox

### Improvements <a id="improvements"></a>

* \[[COMMANDBOX-391](https://ortussolutions.atlassian.net/browse/COMMANDBOX-391)\] - Add Offline Ability for cfengine Server Start
* \[[COMMANDBOX-401](https://ortussolutions.atlassian.net/browse/COMMANDBOX-401)\] - Add box.json data to pre/post publishing interceptors

