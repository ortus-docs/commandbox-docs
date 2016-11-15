# Misc Settings

These are some one-off settings that doen't really belong anywhere else.

## nativeShell
**string**

This setting affects how CommandBox invokes the shell for the `run` command or when using the `!binary` shortcut.  The default *nix shell used for the `run` command is `/bin/sh` but you can override it to use a custom shell.  Set the full path to the shell binary.
```bash
config set nativeShell=/bin/zsh
config show nativeShell
```
## tagVersion
**boolean**

Running the `bump` command from a Git repo will attempt to tag the repo unless you provide the `tagVersion` parameter.  This setting provides a global default to prevent CommandBox from trying to tag Git repos.
```bash
config set tagVersion=false
config show tagVersion
```
## tagPrefix
**string**

Running the `bump` command from a Git repo will tag the repo using the format `v{version}` such as `v1.0.0` or `v4.3.6`.  You can remove the `v` or swap it for another prefix using the `tagPrefix` parameter.  Remember, another string like `foo1.2.3` will not be parseable by CommandBox as a valid semver.  This setting can be overriden by the `tagPrefix` parameter to the `bump` command.
```bash
config set tagPrefix=''
config show tagPrefix
```