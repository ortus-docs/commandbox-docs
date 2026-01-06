---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/Bw6M3PI3e5HgLVcKZz0G/release-history/6.x-versions/whats-new-in-6.2.0
---

# What's New in 6.2.0

This release contains 9 tickets-- several bug fixes and a couple nice new features.

### Jakarta Servlet support

Adobe CF 2025, Lucee 7, and BoxLang 1.0 are all using the Jakarta servlet spec now.  CommandBox will now detect that and automatically download a Jakarta version of Runwar to start the server on first start.

### Server Warmup URLs

CommandBox servers now support one or more warmup URLs which will be automatically fired after the server comes online to ensure it's warmed up, even if there is no external traffic.  You can choose what happens to incoming requests prior to the completion of the warmup (queue, block, or allow) as well  as whether the warmup URLs are fired sync or async.  The warmup URL can also hit any endpoint, acting as a simple webhook to notify that the server has come online if you want.

Read more: [https://ortussolutions.atlassian.net/browse/COMMANDBOX-1391](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1391)

### Release Notes

Here are all the tickets closed in the 6.2.0 release.

#### Improvement

[COMMANDBOX-1645](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1645) Capture partial task output in exception extended info

[COMMANDBOX-1646](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1646) Improve 32 bit detection in java endpoint

[COMMANDBOX-1647](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1647) whitelist /.well-known/ in extension whitelist

[COMMANDBOX-1650](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1650) Support Jakarta EE

#### Bug

[COMMANDBOX-1618](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1618) Coldbox Create Resource - 'open' variable doesn't Exists

[COMMANDBOX-1649](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1649) SSL redirect not working with binding syntax

[COMMANDBOX-1652](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1652) x86 being detected as 32 bit, but it can be 64 bit in java service

[COMMANDBOX-1657](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1657) websockets hitting wrong site in multisite on Adobe

#### New Feature

[COMMANDBOX-1391](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1391) Add on server start URL to warm up the server
