# ForgeBox Distribution

If you package your custom commands and upload them to [ForgeBox](http://www.coldbox.org/forgebox), make sure to tag them as `commandbox-modules` type.  This will tell CommandBox automatically to install them in the custom modules folder so the commands will be automatically registered.

Commands can be installed using the `install` command.  They will not be saved as dependencies.

```bash
install commandbox-chuck-norris
```

The CommandBox shell will be reloaded after a command is installed so it should be ready to use right away.

Here are some example CommandBox Commands:

* [Chuck Norris](http://www.coldbox.org/forgebox/view/CommandBox-Chuck-Norris) ([code](https://github.com/bdw429s/CommandBox-Chuck-Norris))
* [Image To ASCII](http://www.coldbox.org/forgebox/view/CommandBox-ImageToASCII) ([code](https://github.com/bdw429s/CommandBox-Image-To-ASCII))