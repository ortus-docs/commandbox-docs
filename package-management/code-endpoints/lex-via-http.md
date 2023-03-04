---
description: Lucee Extensions
---

# Lex (via HTTP or File)

If you have external Lucee Extensions that need downloaded into your Lucee Server, you can use the `lex:` endpoint to download them. The lex endpoint does not expect the download to be contained in a zip file or to have a box.json. As such, there is no real package slug or name, so CommandBox will "guess" the name based on the name of the lex based on the URL.

```
install lex:https://server.com/path/to/ortus-redis-cache-1.4.0.lex
```

The Lex endpoint can also be used to install local lex files.

```bash
install lex:C:\path\to\my.lex
install lex:/path/to/my.lex
# Windows UNC path
install lex:\\\\serverName\sharename\my.lex
```

Please note that if a Lucee extension is already hosted on ForgeBox, you do not need the Lex extension, as you can simply use the ForgeBox slug just like you would any other page and get the same result but with the addition of semantic versioning.

```
install 5C558CC6-1E67-4776-96A60F9726D580F1
```

## Installation Path

Files from the `lex:` endpoint will be placed in a folder named the same as the package by default unless you provide another folder for installation.  If the current working directory is found to have a Lucee server in it, the lex file will instead be installed to the server context's deploy folder.  Note the Lucee server will need to have been started at least once so CommandBox "knows" about it, but it need not be running at the time. &#x20;

## In box.json

You can specify a lex as dependencies in your `box.json` in this format.

```javascript
{
    "dependencies":{
        "ortus-redis-cache-1.4.0":"lex:https://server.com/path/to/ortus-redis-cache-1.4.0.lex"
    }
}
```

This means you can run a `box install` on a fresh project to pull down extensions needed for your app to run.
