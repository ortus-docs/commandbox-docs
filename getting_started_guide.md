# CommandBox Getting Started Guide

Congratulations on your choice of CommandBox, the next generation of CFML productivity tooling!  We're pleased you've chosen this product and we can't wait to help you get started with it.  Setup is easy and painless.  We'll walk you through the steps you need to become the jealous rage of your peers with the class of a Java guru, the hipster appeal of a Rubyist, and the ASCII art fetish of a Node.js developer.

<center>
    <img src="images/getting_started/in_the_package.png" alt="In The Package">
</center>
<br>

Your CommandBox download was quality checked and  shipped from our integration server with the following items.  You'll want to check the contents of the package to ensure you received everthing.
* CLI
* Package Manager
* Embedded CFML Server
* REPL
* Built-in Help
* ASCII Art

## 1. Download
<img src="images/getting_started/the_package.png" alt="Open the package">

If you don't already have CommandBox in hand, download it from the product page on the Ortus Solutions site:
* [http://www.ortussolutions.com/products/commandbox](http://www.ortussolutions.com/products/commandbox#download)
 
If you already have Java 1.6 or higher installed on your PC, choose the **No JRE Included** download for your operating system.  Otherwise, you can grab the **With JRE Included** for a single-download solution.

You're well on your way now.   While you wait for arrival you might want to secure any loose hair or shirt sleeves and clear a clean space to work on your desktop.  Safety first!


## 2. Unzip & First Run
<img src="images/getting_started/open_package.png" alt="Setup">

Your CommandBox is sent to you via a zip archive.  Decompress the archive to a location of your choice.  The **No JRE Included** download will only have one file in it named `box`.  For Windows users, this will be an `exe` file.  For unix-based users, it will be an executable binary.The **With JRE Included** version will have a `jre` folder.  Keep that folder in the same location as the executable so it can be found.

<img src="images/getting_started/box_icon.png" alt="CommandBox Icon">

Now just double click the file from your GUI, or execute it via a console window.  This will start a short, quick, one-time process of unpacking CommandBox into your user's home directory.  Congratulations, CommandBox is now installed!  You'll still run the same eecutable binary everytime you want to use the CLI, but the extraction process won't need to happen again.

<img src="images/getting_started/first_run.png" alt="First Run">

The green `CommandBox>` prompt is what we call the *interactive shell*.  Type `exit` to close the window or be returned to your OS's native shell.  

## 3. Setup & Usage
<img src="images/getting_started/run.png" alt="Start Using">

To open up the interactive shell at any time, just double click on the `box` executable.  If you prefer to stay in your OS's native shell, then just place the `box` file in your system path and add it before any CommandBox commands like so:

```bash
C:\ > box version
CommandBox 1.0.0+00215
C:\ > _ 
```

The rest of this guide, however, will assume you're sitting at the interactive shell, where you can enjoy cross-platform command consistency, custom history, and tab completion.

The first command you'll want to try out is `help`.  Type it after a command, or even a partial command to help context-specific assistance.Check out the help for the `version` command and then run it to see what you get.

```bash
CommandBox> version help
```

Now, let's see if your installation is up to date with the `upgrade` command:

```bash
CommandBox> upgrade
```

### Package Manager
<img src="images/getting_started/package_manager.png" alt="Embedded Server">

### Embedded Server
<img src="images/getting_started/embedded_server.png" alt="Embedded Server">



### Extensibility
<img src="images/getting_started/extensbility.png" alt="Extensbility">




