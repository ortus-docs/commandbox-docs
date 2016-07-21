# Shell Settings

This setting affects how CommandBox invokes the shell for the `run` command or when using the `!binary` shortcut.

## nativeShell
**string**
The default *nix shell used for the `run` command is `/bin/sh` but you can override it to use a custom shell.  Set the full path to the shell binary.
```bash
config set nativeShell=[\"/var/my/external/modules\"]
config show nativeShell
```