# Jar \(via HTTP\)

If you have external jars that need downloaded into your project, you can use the `jar:` endpoint to download them. The jar endpoint does not expect the jars to be contained in a zip file or to have a box.json. As such, there is no real package slug or name, so CommandBox will "guess" the name based on the name of the jar \(if a jar name appears in the path or query string\).

```text
install jar:http://site.com/path/to/file.jar
install "jar:https://github.com/coldbox-modules/cbox-bcrypt/blob/master/modules/bcrypt/models/lib/jbcrypt.jar?raw=true"
install "jar:https://search.maven.org/remotecontent?filepath=jline/jline/3.0.0.M1/jline-3.0.0.M1.jar"
```

## Installation path

Files from the `jar:` endpoint will be placed in a `lib/` folder by default unless you provide another folder for installation.

## In box.json

You can specify jars as dependencies in your `box.json` in this format.

```javascript
{
    "dependencies":{
        "jline-3.0.0.M1":"jar:https://search.maven.org/remotecontent?filepath=jline/jline/3.0.0.M1/jline-3.0.0.M1.jar"
    }
}
```

Note this installation method does not include any dependencies of the jar like a Maven installation would. That will be a future endpoint.

## Semantic Versioning

The jar endpoint will make the following assumptions about what version of a jar a particular URL may point to:

* It tries and parse a semantic version from the URL's path
* It will store that version in the package's box.json \(`0.0.0` if not found\)
* It will use that version when checking for updates to the jar

This will be reflected in what you see in the **package list** and **outdated** commands.

So for example, if you install a jar dependency like so:

```bash
install jar:https://repo1.maven.org/maven2/co/elastic/apm/elastic-apm-agent/1.24.0/elastic-apm-agent-1.24.0.jar
```

CommandBox will parse out the `1.24.0` from the path of the URL and will assume that as the version of the package.  When checking for outdated dependencies, that version will be used to compare to the current URL in your `box.json` which means an unchanged URL will know it is not out of date.

