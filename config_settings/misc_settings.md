# Misc Settings

These are some one-off settings that doen't really belong anywhere else.

## nativeShell
**string**

This setting affects how CommandBox invokes the shell for the `run` command or when using the `!binary` shortcut.  The default *nix shell used for the `run` command is `/bin/sh` but you can override it to use a custom shell.  Set the full path to the shell binary.
```bash
config set nativeShell=/bin/zsh
config show nativeShell
```