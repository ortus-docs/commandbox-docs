# Custom Tray Menus

When you start a server with CommandBox, we put the relevant options into the right click menu on your system tray icon.  Note, these options differ based on the CF Engine you're using (or if you're using a CF Engine at all, vs a Java WAR app).

You can customize these tray menus and add your own option for your convenience.  For each menu item, you can control the following items:
* `label` : The text that shows in the menu
* `action` : One of the following:
* * `openbrowser` : Open a URL
* * `stopserver` : Stop the server
* `url` : Use in combination with `action="openbrowser"`.  The URL to open.
* `disabled` : Pass `true` and the menu option will be grayed out. Mutually exclusive with `action`.
* `image` : An absolute path to an image to display on the left side of the menu label.


There are 3 ways you can modify the menu options

