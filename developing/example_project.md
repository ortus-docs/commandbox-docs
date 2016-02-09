# Example Project

Here is an example CommandBox module that uses an interceptor (in the `ModuleConfig.cfc`) and custom commands to help manage its settings. 

**On Forgebox:**

[http://www.coldbox.org/forgebox/view/commandbox-banner-customizer](http://www.coldbox.org/forgebox/view/commandbox-banner-customizer)

**On GitHub:**

[https://github.com/bdw429s/commandbox-banner-customizer](https://github.com/bdw429s/commandbox-banner-customizer)

# CommandBox Banner Customizer

Ever gotten tired of the same lame ASCII art every time you start the CommandBox shell in interactive mode?  Well fear not, this module is for you!

## Installation

Install the module like so:
```bash
CommandBox> install commandbox-banner-customizer
```

Once you've done that, there are three module settings you can override to affect how the CLI starts up.

### Hide the banner

```bash
CommandBox> banner off
```
Now reload the CLI and the ASCII art is gone.

### Show the banner

```bash
CommandBox> banner on
```
Now reload the CLI and the ASCII art is back.

### Custom banner text

```bash
CommandBox> banner text "I like spam!"
```
Now reload the CLI and your custom message will appear to greet you.  Note this setting is ignored if the banner is `off`.

### Custom banner file

Want to get all kinds of funky and create your own ASCII art of your cat?  Save the exact text you want to output (can be multiple lines) into a file and specify the file path here:
```bash
CommandBox> banner file myCustomBanner.txt
```
Note this setting is ignored if the banner is `off` or if `banner text` is set.  For extra credit, try including ANSI escape codes in your text file for a color-coded banner

## Advanced

All these example commands do is set config settings for you.  You can also control the module settings without the `banner` commands by directly setting the config settings like so:


```bash
CommandBox> config set modules.commandbox-banner-customizer.hidebanner=true
CommandBox> config set modules.commandbox-banner-customizer.bannertext="I like spam!"
CommandBox> config set modules.commandbox-banner-customizer.bannerfilepath="C:\\myCustomBanner.txt"
```

You can see the current state of this modules settings at any time by using `config show` to print it out.


```bash
CommandBox> config show modules.commandbox-banner-customizer
```