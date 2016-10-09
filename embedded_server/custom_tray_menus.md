# Custom Tray Menus

When you start a server with CommandBox, we put the relevant options into the right click menu on your system tray icon.  Note, these options differ based on the CF Engine you're using (or if you're using a CF Engine at all, vs a Java WAR app).

## Menu attributes

You can customize these tray menus and add your own option for your convenience.  For each menu item, you can control the following items:
* `label` : The text that shows in the menu
* `action` : One of the following:
* * `openbrowser` : Open a URL
* * `stopserver` : Stop the server
* `url` : Use in combination with `action="openbrowser"`.  The URL to open.
* `disabled` : Pass `true` and the menu option will be grayed out. Mutually exclusive with `action`.
* `image` : An absolute path to an image to display on the left side of the menu label.

To keep your menus dynamic, you can use the following place holders which will be replaced at runtime.
* `${runwar.host}` : The host of the server
* `${runwar.port}` : The HTTP port of the server


## Integration points
There are 3 ways you can modify the menu options, server.json, config settings, and via a module.

### server.json
To add a menu contribution to an individual server, add the following to your `server.json`:

```
{
  "trayOptions":[
    {
      "label":"Foo",
      "action":"openbrowser",
      "url":"http://${runwar.host}:${runwar.port}/foobar.cfm",
      "disabled":false,
      "image":"/path/to/image.png"
    }
  ]
}
```

Here is an example of adding a menu item from the command line.
```
server set trayoptions ="[{ 'label':'Whoo Hoo', 'action':'openbrowser', 'url':'http://www.google.com' }]"
```

The menu items you put in the array will be added to the bottom of the server menu.

### Config setting
Menu contributions that you add to the server defaults are global and will be added to every server that you start.  Tray menus are cumulative and menus you add to the `server.json` will not overwrite those in Config settings, but instead be added to them.

You can add menus to the config setting server defaults with the `config set` command.

Here is an example of adding a menu item from the command line.
```
server set server.defaults.trayoptions ="[{ 'label':'Whoo Hoo', 'action':'openbrowser', 'url':'http://www.google.com' }]"
```

### Module `onServerStart` interceptor
If you develop a CommandBox module, your code can contribute menu items to servers as they start, but you even get more control than that.  Using the `onServerStart` interceptor, you have full access to the array of menu items.  You can remove existing menu items or even modify existing ones in the array before they get created.  This give you full control of the menu!

An example of this is our FusionReactor module, which adds an "Open FusionReactor" menu item.  The `serverInfo` struct is passed into the `onServerStart` interceptor as part of the `interceptData`.
```
// Add FusionReactor menu item to tray icon.
serverInfo.trayOptions.append(
	{ 'label':'Open FusionReactor', 'action':'openbrowser', 'url':serverInfo.FRURL }
);
```

> **Warning** Please note, the JSON keys are case sensitive and Runwar currently throws weird NPE's if you type them wrong.  Put the keys in quotes when declaring them in CFML to keep the case.