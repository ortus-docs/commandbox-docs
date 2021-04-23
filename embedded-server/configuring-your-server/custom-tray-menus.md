# Custom Tray Menus

For every server you start, there is a corresponding icon in your PC's system tray.  In addition to the menu options you get out of the box, you can add in custom menu items.  Menus are defined as nested arrays of structs, where each struct represents a single menu item.  A menu item can be informational, have an action, or contain sub menus.   

```javascript
[
    {
        "label" : "My Custom Menu Item!",
        "disabled" : true
    }
]
```

You can customize the tray menus for a server via 3 different methods:

* The `trayOptions` array in a specific server's `server.json` file.  These menu additions will be specific to that server.
* The `server.defaults.trayOptions` config setting. These menu additions will be added to every server you start
* A custom CommandBox Module with an interceptor listening to the `onServerStart` interception point that modifies the `serverInfo.trayOptions` array.

Each menu item struct can have the following keys.  Only `label` is required. 

* **label** - This text appears on the menu and is the unique name
* **action** - This controls what the menu item does when clicked.  Possible options are:
  * **openfilesystem** - Opens a folder on the file system.  Requires `path` to be set as well.
  * **openbrowser** - Opens a URL in your default browser. Requires `url` to be set as well.
  * **stopserver** - Stops the current server
  * **run** - Runs an arbitrary native command synchronously. Requires `command` to be set as well.
  * **runAsync** - Runs an arbitrary native command asynchronously  Requires `command` to be set as well. 
  * **runTerminal** - Runs an arbitrary native command in a new Terminal window. Requires `command` to be set as well. 
* **path** - The file system path to use for the `openfilesystem` action.
* **url** - The HTTP URL to use for the `openbrowser` action
* **image** - A custom image file to use for the icon. Relative paths in the `server.json` will be relative to the JSON file  Relative paths in the global config will be relative to the web root of the server.
  * **command** - The native command to run for the `run`, `runAsync`, or `runTerminal` actions.
* **workingDirectory** - The working directory to use for the `run`, `runAsync` or `runTerminal` actions.
* **shell** - Override the native shell to use on your OS.  Defaults to your `nativeShell` config setting.
* **items** - An array that contains a struct of sub menus.
* **disabled** - Boolean that greys out menu item and disables any action.  Use for informational items.

### openfilesystem Example

```javascript
[
    {
        "label" : "Open Downloads",
        "action" : "openfilesystem",
        "path" : "D:/downloads",
        "image" : "C:/pictures/folder.png"
    }
]
```

### openbrowser Example

```javascript
[
    {
        "label" : "Open Google",
        "action" : "openbrowser",
        "url" : "https://www.google.com",
        "image" : "C:/pictures/cat.png"
    }
]
```

## stopserver Example

```javascript
[
    {
        "label" : "Kill it!",
        "action" : "stopserver",
        "image" : "images/bomb.png"
    }
]
```

### run Example

Commands executed by the `run` action will display their output in a popup window.  The PID of the process is available if running on Java 9 or later.  Also, there is a button to kill the process, but your mileage may vary.  Only use this option to run command line apps that have console output you want to see.  

```javascript
[
    {
        "label" : "Does the Internet work?",
        "action" : "run",
        "command" : "ping google.com"
    }
]
```

### runAsync Example

Commands executed by the `runAsync` action work like the `run` action except there is no windows to show you the output.  This is what you want to use to fire off an app such as an IDE.

```javascript
[
    {
        "label" : "Math is math!",
        "action" : "runAsync",
        "command" : "calc.exe"
    }
]
```

### runTerminal  Example

Commands executed by the `runTerminal` action work like the `run` action except a new terminal window is opened to run the command.  The terminal windows stays open and you can continue to interact with it when the command finishes.  

```javascript
[
    {
        "label" : "Update dependencies",
        "action" : "runTerminal",
        "command" : "box update"
    }
]
```

You can customize how your command is run by setting the optional `workingDirectory` and `shell` properties.  If not set, the working directory will be the web root of the server.

```javascript
[
    {
        "label" : "Clear Temp Dir",
        "action" : "runAsync",
        "command" : "rm -rf",
        "workingDirectory" : "/var/tmp",
        "shell" : "/bin/zsh"
    }
]
```

## Sub Menus

You can nest menus by supplying another array of structs in the `items` property of a menu.  A menu cannot have an action and have sub menus.

```javascript
[
    {
        "label" : "Top menu",
        "items" : [
            {
                "label" : "Sub 1"
            },
            {
                "label" : "Sub 2"
            },
            {
                "label" : "Sub 3"
            }
        ]
    }
]
```

## Override Existing Menus

Menu items are layered "on top" of existing options whichs mean you can add new sub menu items to an existing top level menu.  You can even override the action or image of a built-in menu.  Menu items are registered in this order:

* Built-in menus
* Config setting server defaults
* server.json trayOptions
* onServerStart interceptor

This example `server.json` will add a new sub menu into the existing "Open..." top level menu.

```javascript
{
    "trayOptions":[
        {
            "label":"Open...",
            "items":[
                {
                    "action":"runAsync",
                    "command":"calc.exe",
                    "label":"Calculator"
                }
            ]
        }
    ]
}
```

There is no way to remove existing menu items via your config settings or `server.json`. To do that, you'd need to directly manipulate the `trayOptions` array in an interceptor.

