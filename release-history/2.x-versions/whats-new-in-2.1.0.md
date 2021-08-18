# What's New in 2.1.0

### Specifying a max heap size for your embedded server

```bash
CommandBox> start heapSize=768
```

And finally, the ability to install packages from a Git repo is here!

```bash
install git://github.com/username/repoName.git
```

And this nice shortcut for installing from GitHub:

```bash
install username/repoName
```

If you've not used CommandBox yet, check out our getting started guide here:

[http://commandbox.ortusbooks.com/content/getting\_started\_guide.html](http://commandbox.ortusbooks.com/content/getting_started_guide.html)

You can download CommandBox 2.1.0 here on our product page:

[http://www.ortussolutions.com/products/commandbox\#download](https://www.ortussolutions.com/products/commandbox#download)

## Release Notes

### Bug

* \[[COMMANDBOX-170](https://ortussolutions.atlassian.net/browse/COMMANDBOX-170)\] - Piping content to box repl
* \[[COMMANDBOX-220](https://ortussolutions.atlassian.net/browse/COMMANDBOX-220)\] - Lucee default error template is broken in CLI
* \[[COMMANDBOX-221](https://ortussolutions.atlassian.net/browse/COMMANDBOX-221)\] - OWASP jars corrupt
* \[[COMMANDBOX-229](https://ortussolutions.atlassian.net/browse/COMMANDBOX-229)\] - REPLParser doesn't allow // in strings \(like in URLs\)
* \[[COMMANDBOX-246](https://ortussolutions.atlassian.net/browse/COMMANDBOX-246)\] - commandbox.properties isn't picked up when box.exe is in a folder with spaces

### Improvement

* \[[COMMANDBOX-165](https://ortussolutions.atlassian.net/browse/COMMANDBOX-165)\] - Improve parameter escaping when running commands from native OS
* \[[COMMANDBOX-210](https://ortussolutions.atlassian.net/browse/COMMANDBOX-210)\] - Make ColdBox skeleton hints dynamic by reading folder instead of hard-coded values
* \[[COMMANDBOX-244](https://ortussolutions.atlassian.net/browse/COMMANDBOX-244)\] - Increase the size/resolution of the icon

### New Feature

* \[[COMMANDBOX-65](https://ortussolutions.atlassian.net/browse/COMMANDBOX-65)\] - Additional install endpoints
* \[[COMMANDBOX-240](https://ortussolutions.atlassian.net/browse/COMMANDBOX-240)\] - Git endpoint
* \[[COMMANDBOX-242](https://ortussolutions.atlassian.net/browse/COMMANDBOX-242)\] - Update all tray icons to CommandBox latest icons
* \[[COMMANDBOX-243](https://ortussolutions.atlassian.net/browse/COMMANDBOX-243)\] - new server argument: heapSize to allow for sizing the embedded server heap size
* \[[COMMANDBOX-256](https://ortussolutions.atlassian.net/browse/COMMANDBOX-256)\] - Integrate the Loader java bits into the CommandBox source
* \[[COMMANDBOX-257](https://ortussolutions.atlassian.net/browse/COMMANDBOX-257)\] - Migrate APIDocs generation to DocBox

