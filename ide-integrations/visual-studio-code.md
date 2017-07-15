# CommandBox Visual Studio Code Integration

Here are suggestions for using **VSC** \(_Visual Studio Code_\) for developing in **CFML**.

What do you want to do with VSC?

1. Install it?
2. Key Extensions?
3. Working with the shell/terminal
4. Git Extensions?

## Installing VSC

This IDE has it's own domain. \( [https://code.visualstudio.com/](https://code.visualstudio.com/) \) with downloads for **macOS**, **Windows** and **Linux** on the home page. :+1:

![Visual Studio Code Snapshot](https://code.visualstudio.com/home/home-screenshot-win-lg.png)

## Using The Terminal/Shell

You can approach this different ways. It is pretty nice to have the command prompt running right inside your IDE. Pressing _CTRL +\`_ will show and/or hide your terminal. This will let you run _Commandbox_ directly inside your terminal.

### Commandbox only

Go to `File > Preferences > Settings` and search for `shell.windows`. Hover over the item in `DEFAULT SETTINGS` and you will see an edit pencil. Click on that and it will copy the values over to `USER SETTINGS`. \(NOTE: You can also set stuff up in `WORKSPACE SETTINGS` for different projects. Now set the location of commandbox on your system like we did in our system in the example below.

`"terminal.integrated.shell.windows": "/path/to/box",`

When adding a path, follow the rules for adding platform specific path seperators.

### Commandbox and Shells of Choice

Install Shell Launcher and reload the IDE.

* [Shell Launcher](https://marketplace.visualstudio.com/items?itemName=Tyriar.shell-launcher) _Easily launch multiple shell configurations in the terminal._

Now open your user settings like is shown in the Commandbox only section just above. Add the following to your `USER SETTINGS`.

`//Shell launcher  
     // A list of shell configurations for Windows  
    "shellLauncher.shells.windows": [  
        {  
        "shell": "C:\\Windows\\sysnative\\cmd.exe",  
        "label": "cmd"  
        },  
        {  
        "shell": "/path/to/box",  
        "label": "Commandbox"  
        }  
    ],`

### Or loading it as desired

If you have commandbox mapped to a path you can of course just call box like you would from any terminal and it will load right up for you. Just use any terminal in VSC that would load `box` outside VSC and it will run the same.

## Key Extensions

You should also install a few extensions. \(These extensions can also be added from the built in Extentions item.\)

* [CFML Language Support](https://marketplace.visualstudio.com/items?itemName=ilich8086.ColdFusion)
* [Tag Comment Support](https://marketplace.visualstudio.com/items?itemName=trst.cfml-comment-tags)

## Git Extensions

Additional Extensions \( of course, choose the ones you want and ignore the others \)

* [Git Lens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) _This will take your git IDE to new levels._

## P.S. A couple of other nice plugins.

* [TODO Highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight) _Add TODO, FIXME or other comments to code and find quickly with this extension._
* [Project Manager](https://marketplace.visualstudio.com/items?itemName=alefragnani.project-manager): save folders as projects and easily switch between projects
* [Hg](https://marketplace.visualstudio.com/items?itemName=mrcrowl.hg): integrated Mercurial source control
* [Great Icons](https://marketplace.visualstudio.com/items?itemName=emmanuelbeziat.vscode-great-icons) _Because it is more fun, nuff said!_

More information to follow. \(Including video guides to show how to get started that may not be as familiar with different parts.\)

