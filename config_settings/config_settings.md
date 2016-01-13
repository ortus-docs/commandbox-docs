# CommandBox Config Settings

CommandBox has a global configuration file that stores user settings.  It is located in `~/.CommandBox/CommandBox.json` and can be used to customize core CommandBox behaviors as well as overriding module settings.  Config settings are managed by the `config set`, `config show`, and `config clear` commands.

Set a config setting
```bash
config set mySetting=value
```

View the setting:

```bash
config show mySetting
```

Remove the saved setting:

```bash
config clear mySetting
```