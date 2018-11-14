# Upgrading

CommandBox is a Java-based tool that involves several pieces including native Java classes, CFML code, and the embedded Railo CLI. However, most changes are confined to the CFML code managed by [WireBox](http://wiki.coldbox.org/wiki/WireBox.cfm). To determine what version you have installed, use the `version` command.

```bash
box version
```

The auto-upgrade commands shown below will **not** upgrade the main CommandBox Java Binary. If there are any major upgrades to this binary, you will see a message that you will need to download the new Java binary and replace your current one.

## Stable

To upgrade to the last stable version of the shell and commands, use the `upgrade` command.

```bash
box upgrade
```

This command will connect to our server to determine the last stable build. If there is an upgrade, it will be downloaded and installed for you.

## Bleeding Edge

To upgrade to the bleeding edge version of the shell and commands, use the **latest** flag.

```bash
box upgrade --latest
```

This command will connect to our server to determine the latest build. If there is an upgrade, it will be downloaded and installed for you.

## Use The Force

If you already have the latest version installed, but you still want to force an upgrade, use the **force** parameter.

```bash
upgrade --force
upgrade --latest --force
```

## Brute Force

Note, if you delete your `{user}/.CommandBox` folder and re-run the executable, the version of CommandBox in the executable will be unpacked regardless of any upgrades you may have installed in the mean time. On that note, another way to force an upgrade is to simply download the new executable, wipe the `.CommandBox` folder in your user directory and re-run. This will also erase any saved command history, embedded servers, or installed user commands. servers, or installed user commands.

## Mac/  \*Unix

If you have used Hombrew to install CommandBox you must use Homebrew for any upgrade, minor or major. To upgrade CommandBox with Homebrew:

```brew uninstall commandbox
brew install commandbox```
