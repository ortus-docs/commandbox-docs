# Linking Modules

CommandBox allows you to link a module directly into the CommandBox core so that you can use the module as though it's installed but without having two copies of the code. To do this, `cd` to the root of the module you want to link into CommandBox.

```bash
cd /path/to/my/cool/module
package link
```

This will create a symlink in the core CommandBox modules directory to your package and reload the shell for you so you can immediately test it out. Any further code changes you make can be tested by running the `reload` command again.

## Unlinking

When you are done testing, you can remove the link by running the `package unlink` command from the root of the module you've been developing on.

```bash
cd /path/to/my/cool/module
package unlink
```

This will remove the symlink for you.
