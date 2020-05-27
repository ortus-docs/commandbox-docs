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

## Additional actions available to create tray icon options

`action` values available

`run:` Use this to execute a command from your tray menu if you are expecting to get some feedback from the result, a small window will pop to show the output for the executed command.

```bash
"label":"Open VScode",
"action":"runAsync",
"command":"code .",
"disabled":false,
"image":"/path/to/image.png"
```

`runAsync:` Use this to execute a command from your tray menu if you are NOT expecting to get any output, this will execute the command in silent mode.

```bash
"label":"Open VScode",
"action":"runAsync",
"command":"code .",
"disabled":false,
"image":"/path/to/image.png"
```

`runTerminal:` Use this to execute a command in a separated terminal window, the window will not close when the actions is done.

```bash
"label":"Say Hello",
"action":"runTerminal",
"command":"echo " hello "",
"disabled":false,
"image":"/path/to/image.png"
```

Also the new property `workingDirectory` has been added to the `trayOption` definition, this property allows to set the current working directory for all the executed commands i.e.

```bash
"label":"Start an awesome app",
"action":"run",
"command":"box server start",
"workingDirectory":"/path/to/my/app/folder"
"disabled":false,
"image":"/path/to/image.png"
```

## Defining a custom shell to use arbitrary actions

To define a custom shell to use arbitrary actions on tray menu, Commandbox has the ability to determinate an available shell to use, however it is possible to set a particular shell setting a property to your `server.json`

```bash
server set defaultShell="/bin/bash"
```

If you start your server using  `--trace` you can check the parameter been set.