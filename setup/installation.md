# Installation

Regardless of where you place the **box** binary, the first time you execute
it, a `.CommandBox` folder will be created in your user's home
directory and CommandBox will be extracted into that location. If you
delete this directory, it will be replaced the next time the CommandBox
executable is run.

Windows
-------

Unzip the executable **box.exe** and just double click on it to open the
shell. When you are finished running commands, you can just close the
window, or type <kbd>exit</kbd>.

<div class="alert alert-success">
**Tip**: You can make the <kbd>box</kbd> available in any Windows
terminal by adding its location to the **PATH** system environment
variable. See [1][]

</div>
Mac/\*Unix
----------

### Manual Installation

Unzip the binary **box** and just double click on it to open the shell.
When you are finished running commands, you can just close the window,
or type <kbd>exit</kbd>.

<div class="alert alert-success">
**Tip**: You can place the binary in your `/usr/bin` directory so it can
be available system-wide via the <kbd>box</kbd> command in any terminal
window.

</div>
### Homebrew (Mac)

Homebrew is a great Mac package manager, it can easily install and keep
your CommandBox installation up to date, just run the following:

    <nowiki>
    brew install https://github.com/Ortus-Solutions/commandbox/raw/master/build/commandbox.rb
    </nowiki>

<div class="alert alert-info">
Once we reach stable version, we will be submitting CommandBox to the
Homebrew Binary Tap, for even easier installation.

</div>
Linux (Redhat)
--------------

After you have downloaded the .rpm file, install it using the “rpm”
command.

    rpm –ivh commandbox-rpm-1.0.0.rpm

Run the <kbd>box</kbd> binary to begin the one-time unpack process.

Linux (Debian)
--------------

After you have downloaded the .deb file, install it using the “dpkg”
command.

    sudo dpkg -i commandbox-debian-1.0.0.deb

Run the <kbd>box</kbd> binary to begin the one-time unpack process.

  [1]: http://www.computerhope.com/issues/ch000549.htm