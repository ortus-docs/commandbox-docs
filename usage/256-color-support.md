# 256 Color Support

CommandBox has support for 256 colors in the console, but this is limited by the terminal in use.  For instance, SSHing into a Linux server with PutTTY only supports 8 colors.  Windows cmd only supports 16 colors.  Most Mac terminals seem to support 256 colors by default.  

For Windows users, we recommend using an add-on terminal like ConEMU which has good 256 color support out of the box.  To find out how many colors your terminal supports, you can run this command:

```text
system-colors
```

![256 Color support from ConEMU in Windows](../.gitbook/assets/image%20%282%29.png)

This will show you at the top how many colors are supported.  It will also output a sample of each of the 256 colors.  Terminals that support less than 256 colors will "round" down and show the next closest color automatically.  Some darker colors might turn to black.  Note, some advanced terminals allow the user to choose color themes which will also change the default colors.  CommandBox has no control over how colors show up for you.

## Color Names

The names and numbers of each color are unique and important if you want to do any Task Runners, custom commands that make use of these colors.  Modules like Bullet Train also allow you to customize their colors.  You can specify a color by its name like `LightGoldenrod5` or its number \(`221`\).

In a Task Runner or custom command, the print helper would look like this:

```text
print.lightGoldenrod5Line( 'This is pretty' );
print.color221( 'This is the same color as above' );
```

Here's an example of customizing your Bullet Train module to use fancy colors.  \(Color 21 is `Blue1`\)

```text
config set modules.commandbox-bullet-train.packageBG=lightGoldenrod5
config set modules.commandbox-bullet-train.packageText=color21
```



