# Misc Settings

These are some one-off settings that doesn't really belong anywhere else.

## nativeShell

**string**

This setting affects how CommandBox invokes the shell for the `run` command or when using the `!binary` shortcut. The default \*nix shell used for the `run` command is `/bin/sh` but you can override it to use a custom shell. Set the full path to the shell binary.

```bash
config set nativeShell=/bin/zsh
config show nativeShell
```

## tagVersion

**boolean**

Running the `bump` command from a Git repo will attempt to tag the repo unless you provide the `tagVersion` parameter. This setting provides a global default to prevent CommandBox from trying to tag Git repos.

```bash
config set tagVersion=false
config show tagVersion
```

## tagPrefix

**string**

Running the `bump` command from a Git repo will tag the repo using the format `v{version}` such as `v1.0.0` or `v4.3.6`. You can remove the `v` or swap it for another prefix using the `tagPrefix` parameter. Remember, another string like `foo1.2.3` will not be parseable by CommandBox as a valid semver. This setting can be overridden by the `tagPrefix` parameter to the `bump` command.

```bash
config set tagPrefix=''
config show tagPrefix
```

## artifactsDirectory

**string**

You can control where your artifact cache is stored with the `artifactsDirectory` config setting. This can be useful to keep your primary drive from filling up, or to point your files to a shared network drive that your coworkers can share.

```bash
config set artifactsDirectory=/path/to/artifacts
config show artifactsDirectory
```

## colorInDumbTerminal

**boolean**

You can enable this setting if you want to force CommandBox to output ANSI formatting code even though you're running box inside of a non-interactive terminal. This is handy for CI builds such as Gitlab, which will process color coded text in your job logs. Default value is `false`.

```bash
config set colorInDumbTerminal=true
config show colorInDumbTerminal
```

## preferredBrowser

**string**

Used to override the default browser to open when a server starts, or when using a command like `server open` or calling the `openURL()` method from a command or Task Runner. Possible values are:

* **firefox**
* **chrome**
* **opera**
* **edge** (Windows and Mac only)
* **ie** (Windows only)
* **safari** (Mac only)
* **konqueror** (Linux only)
* **epiphany** (Linux only)

```bash
config set preferredBrowser=chrome
config show preferredBrowser
```

## tabCompleteInline

**boolean**

You can change CommandBox's default tab completion to be an inline list that follows your cursor. **This setting requires you to close and re-open the shell to take affect.**

```bash
config set tabCompleteInline=true
config show tabCompleteInline
```

![](<../.gitbook/assets/image (14).png>)

## developerMode

The `developerMode` setting reloads shell before each command to help testing changes to CommandBox core or modules.

```
config set developerMode=true
```

It will prevent you from needing to use the `reload` command, but it will cause a delay before each command. Don't forget to turn this back off when you're done.

## terminalWidth

Terminal width can be overridden for entire CLI. This will affect ASCII art, interactive job output, progress bars, and the table printer.

```
config set terminalWidth=150
```

## offlineMode

The `offlineMode` setting will disable most external HTTP calls. This can be useful for

* testing production server starts to ensure they aren’t reliant on external calls
* running `box` in a secure network which blocks or flags any external access

```bash
# enable offline mode
config set offlineMode=true

# go back to normal
config set offlineMode=falses
```

This setting is obeyed in the following parts of CommandBox:

* commandbox-update-check module
* installation endpoints
  * forgebox
  * http/https/cached+http/cached\_https
  * git/git+ssh/git+https/github
  * java
  * lex
  * jar
  * CFLib
  * S3
* upgrade command
* inside the progressible downloader class
