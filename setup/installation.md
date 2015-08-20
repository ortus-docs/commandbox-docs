# Installation

Regardless of where you place the **box** binary, the first time you execute
it, a `.CommandBox` folder will be created in your user's home
directory and CommandBox will be extracted into that location. If you
delete this directory, it will be replaced the next time the CommandBox
executable is run. 

You can specify a different install location by adding `-commandbox_home=E:\CommandBox` when you run the box binary.

To avoid specifying the commandbox_home variable every time you can create a file 
called `commandbox.properties` (case sensitive) in the same directory as 
the binary, and fill it with this line: 

```bash
commandbox_home=E:\\CommandBox 
```

## <i class="fa fa-windows"></i> Windows

Unzip the executable **box.exe** and just double click on it to open the
shell. When you are finished running commands, you can just close the
window, or type `exit`.

>**Hint** You can make the `box.exe` available in any Windows
terminal by adding its location to the `PATH` system environment
variable. See http://www.computerhope.com/issues/ch000549.htm


## <i class="fa fa-apple"></i><i class="fa fa-linux"></i> Mac/\*Unix

### Manual Installation

Unzip the binary **box** and just double click on it to open the shell terminal.
When you are finished running commands, you can just close the window,
or type `exit`.

>**Hint** You can place the binary in your `/usr/bin` directory so it can
be available system-wide via the `box` command in any terminal
window.

### Homebrew (Mac)

[Homebrew](http://brew.sh) is a great Mac package manager, it can easily install and keep
your CommandBox installation up to date (even binary releases), just run the following for stable releases:

```bash
brew install http://downloads.ortussolutions.com/ortussolutions/commandbox/commandbox.rb
```

And the following for bleeding edge releases:

```bash
brew install http://integration.stg.ortussolutions.com/artifacts/ortussolutions/commandbox/commandbox-be.rb
```


## Linux (Redhat)

After you have downloaded the `commandbox.rpm` file, install it using the `rpm`
command.

```bash
rpm –ivh commandbox-rpm-1.2.3.rpm
```

Then run the `box` binary to begin the one-time unpacking process.

## Linux Yum
Add the following to: `/etc/yum.repos.d/commandbox.repo`

```
[CommandBox]
name=CommandBox $releasever - $basearch
failovermethod=priority
baseurl=http://downloads.ortussolutions.com/RPMS/noarch
enabled=1
metadata_expire=7d
gpgcheck=0
```

Then run:

```bash
sudo yum update && install commandbox
```

Then run the `box` binary to begin the one-time unpacking process.

## Linux APT

Add the following to: `/etc/apt/sources.list.d/box.list`

**Stable**
```bash
deb http://downloads.ortussolutions.com/debs/noarch /
```

**Bleeding Edge**
```bash
deb http://integration.stg.ortussolutions.com/artifacts/debs/noarch /
```

Then run:

```bash
sudo apt-get update && apt-get install commandbox
```

Then run the `box` binary to begin the one-time unpacking process.

## Linux (Debian)

After you have downloaded the `commandbox.deb` file, install it using the `dpkg`
command.

```bash
sudo dpkg -i commandbox-debian-1.2.3.deb
```

Run the `box` binary to begin the one-time unpacking process.

  [1]: http://www.computerhope.com/issues/ch000549.htm
