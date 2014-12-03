# Upgrading

CommandBox is a Java-based tool that involves several pieces including
native Java classes, CFML code, and the embedded Railo CLI. However, most changes
are confined to the CFML code managed by [WireBox](http://wiki.coldbox.org/wiki/WireBox.cfm). To determine what version you have
installed, use the `version` command.

```bash
box version
```

## Stable

To update to the last stable version of the shell and commands, use the
`upgrade` command.

```bash
box upgrade
```

This command will connect to our server to determine the last stable
build. If there is an update, it will be downloaded and installed for
you.

Bleeding Edge
-------------

To update to the bleeding edge version of the shell and commands, use
the **latest** flag.

    upgrade --latest

This command will connect to our server to determine the latest build.
If there is an update, it will be downloaded and installed for you.

Use The Force
-------------

If you already have the latest version installed, but you still want to
force an update, use the **force** parameter.

    upgrade --force

    upgrade --latest --force

Brute Force
-----------

Note, if you delete your `{user}/.CommandBox` folder and re-run the
executable, the version of CommandBox in the executable will be unpacked
regardless of any updates you may have installed in the mean time. On
that note, another way to force an upgrade is to simply download the new
executable, wipe the `.CommandBox` folder in your user directory and
re-run. This will also erase any saved command history, embedded
servers, or installed user commands.