# HTTP(S) Endpoint

Packages that are either stored locally on your machine or are accessable via a network drive can be install 

## Installation

Every package on ForgeBox has a unique slug.  To install a package, use the slug like so:

```bash
install cborm
```
You can also specify the version of a package you want to install from Forgebox. Note, this only 
currently works if the specified version of the package is in your local artifacts folder.  

```bash
install coldbox@3.8.1
```

Given the install command above, if the file `~/.CommandBox/artifacts/coldbox/3.8.1/coldbox.zip` exists on your hard drive, the installation will not connect to Forgebox at all.  It will be a completely offline installation.


## In box.json

You can specify packages from ForgeBox as dependencies in your `box.json` in this format:

```javascript
{
    "dependencies" : {
        "coldbox" : "4.1.0"
    }
}

```
