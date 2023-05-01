# What's New in 5.4.2

* There is a fix for a regression introduced in 5.4.0 where updating the version of a CF engine doesn't work without forgetting the server first.&#x20;
* There is also an important security improvement to CommandBox servers.  Thanks to Abram Adams for reporting this to Ortus so we could address it. &#x20;

## Release notes

Note, the details of the security improvement have been tracked privately.

#### Bug

[COMMANDBOX-1382](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1382) Java path shows up twice in "server info --verbose"

[COMMANDBOX-1381](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1381) Updating server in-place keeps old web.xml path

[COMMANDBOX-1375](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1375) recipe with multiple "install" instructions fails

#### Improvement

[COMMANDBOX-1380](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1380) Add additional interceptData to server interceptors

[COMMANDBOX-1379](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1379) Update to WireBox 6.5.2

[COMMANDBOX-1376](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1376) Immediately activate modules after installation

[COMMANDBOX-1349](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1349) Improve multiselect DSL

#### Story

[COMMANDBOX-1120](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1120) Add JSON and Properties output for info command
