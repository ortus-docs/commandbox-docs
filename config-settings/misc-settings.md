# Misc Settings

These are some one-off settings that doen't really belong anywhere else.

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

Running the `bump` command from a Git repo will tag the repo using the format `v{version}` such as `v1.0.0` or `v4.3.6`. You can remove the `v` or swap it for another prefix using the `tagPrefix` parameter. Remember, another string like `foo1.2.3` will not be parseable by CommandBox as a valid semver. This setting can be overriden by the `tagPrefix` parameter to the `bump` command.

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

You can enable this setting if you want to force CommandBox to output ANSI formatting code even though you're running box inside of a non-interactive terminal.  This is handy for CI builds such as Gitlab, which will process color coded text in your job logs.  Default value is `false`.

```bash
config set colorInDumbTerminal=true
config show colorInDumbTerminal
```

