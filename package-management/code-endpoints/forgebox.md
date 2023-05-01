# ForgeBox

ForgeBox is an online registry of packages run by Ortus Solutions. The web UI for ForgeBox is located at [http://forgebox.io](http://forgebox.io). Signing up for a ForgeBox account is quick, easy, and free. You will need your own account to post packages, but anyone can browse and install packages anonymously.

## Installation

Every package on ForgeBox has a unique slug. To install a package, use the slug like so:

```bash
install cborm
```

You can also specify the version of a package you want to install from Forgebox.

```bash
install coldbox@3.8.1
```

Given the install command above, if the file `~/.CommandBox/artifacts/coldbox/3.8.1/coldbox.zip` exists on your hard drive, the installation will not connect to Forgebox at all. It will be a completely offline installation. This only works when you type an exact version that includes a major, minor, and patch number.

## In box.json

You can specify packages from ForgeBox as dependencies in your `box.json` in this format:

```javascript
{
    "dependencies" : {
        "coldbox" : "^4.1.0"
    }
}
```

> _info_ The caret `^` means that the `update` command will update minor releases but not major releases.

## Installing Lucee Extensions

If the package being installed from ForgeBox is of type `lucee-extensions` and if the current working directory is found to have a Lucee server in it, the lex file will instead be installed to the server context's deploy folder. Note the Lucee server will need to have been started at least once so CommandBox "knows" about it, but it need not be running at the time.

```
install 5C558CC6-1E67-4776-96A60F9726D580F1
```

## ForgeBox Pro

For companies who want to host internal code endpoints for private packages, we will soon support an Enterprise version of ForgeBox that can be installed behind your company's firewall. Please contact us if this feature interests you.

## ForgeBox namespace

Inside CommandBox, use the **forgebox** namespace to **search** for packages or **show** packages of your choosing.

### forgebox search

The first command to try out is "forgebox search". It takes a single parameter which is a string to perform a case-insensitive search for. Any entry whose title, summary or author name contains that text will be displayed:

```bash
forgebox search awesome
```

### forgebox show

The "forgebox show" command takes several parameters and is pretty flexible. The first way to use it is to just view the details of a single entry using the slug.

```bash
forgebox show coldbox
```

You can get lists of items filtered by package type (modules, interceptors, caching, etc) and ordered by popular, new, or recent. Here's some examples:

```bash
forgebox show plugins
forgebox show new modules
forgebox show recent commandbox-commands
```

Too many results on one page? Use the built-in pagination options:

```bash
forgebox show orderby=new maxRows=10 startRow=11
```

Or just pipe the output into the built-in "more" or "grep" command.

```bash
forgebox show new | more
forgebox show modules | grep brad
```

### forgebox show help

If you have troubles remembering the valid types or order by's, remember you can always hit "tab" for autocomplete within the interactive shell. Adding "help" to the end of any command will also show you the specific help for that command.

```bash
forgebox help
forgebox search help
forgebox show help
```

### forgebox types

The list of types in ForgeBox is dynamic so we don't list them out in the help. Instead, we made a handy "forgebox types" command to pull the latest list of types for you.

```bash
forgebox types
```
