# Box.json

The `box.json` file must be in your root of your project and it is a JSON file that describes your project, dependencies, development dependencies, installation data, and CommandBox command data. 

>**Note** Please note that you can add as many settings or alter the `box.json` structure to meet your needs when developing commands. This makes our descriptor incredibly flexible.

## Initialize a package
To initialize any folder as a package, run the `init` command.  

```bash
init
```

You can pass as many properties to the `init` command as you want using named parameters.

```bash
init name="My Package" slug=my-package version=1.0.0
```

You can also do a question/answer style wizard by adding the `--wizard` flag.

```bash
init --wizard
```

## Sample box.json

Below you will see all the possible options that we currently support in CommandBox. Note, not all have been implemented yet.  If you have suggestions or updates to our package descriptor, please do not hesitate to [Contact Us](https://groups.google.com/a/ortussolutions.com/forum/#!forum/commandbox)!

```javascript
{
    "name" : "Package Name",
    // ForgeBox unique slug
    "slug" : "",
    // semantic version of your package
    "version" : "1.0.0.buildID",
    // author of this package
    "author" : "Luis Majano <lmajano@ortussolutions.com>",
    // location of where to download the package, overrides ForgeBox location
    "location" : "URL,Git/svn endpoint,etc",
    // install directory where this package should be placed once installed, if not
    // defined it then installs were the CommandBox command was executed.
    "directory" : "",
    // This boolean bit determines if the container directory will contain a sub-directory according to
    // the package slug name, the default is true
    "createPackageDirectory" : "boolean",
    // If this is set, then we will use this name for the package sub-directory, instead of the slug name
    "packageDirectory" : ""
    // project homepage URL 
    "Homepage" : "URL",
    // documentation URL
    "Documentation" : "URL",
    // source repository, valid keys: type, URL 
    "Repository" : { 
        "type" : "git,svn,mercurial", "URL" : ""
    },
    // bug issue management URL
    "Bugs" : "URL",
    // ForgeBox short description
    "shortDescription" : "short description",
    // ForgeBox big description, if not set it looks for a Readme.md, Readme, Readme.txt
    "description" : "",
    // Install instructions, if not set it looks for a instructions.md, instructions, instructions.txt
    "instructions" : "",
    // Change log, if not set, it looks for a changelog.md, changelog or changelog.txt
    "changelog" : ""
    // ForgeBox contribution type
    "type" : "1 from forgebox available types",
    // ForgeBox keywords, array of strings
    "keywords" : [ "groovy", "module" ],
    // Bit that if set to true, will not allow ForgeBox posting if using commands
    "private" : "boolean",
    // cfml engines it supports, type and version
    "engines" : [
        { "type" : "railo", "version" : ">=4.1.x" },
        { "type" : "lucee", "version" : ">=4.5.x" },
        { "type" : "adobe", "version" : ">=10.0.0" }
    ],
    // default engine to use using our run embedded server command
    // Available engines are lucee, railo, cf9, cf10, cf11
    "defaultEngine" : "cf9, railo, cf11",
     // default project URL if not using our start server commands
    "ProjectURL" : "http://railopresso.local/myApp",
    // license array of licenses it can have
    "License" : [
        { "type" : "MIT", "URL" : "" }
    ]
    // contributors array of strings or structs: name,email,url 
    "Contributors" : [ "Luis Majano", "Luis Majano <lmajano@mail.com>", {name="luis majano", email="", url=""} ],
    // dependencies, a shortcut for latest version is to use the * string
    "Dependencies" : {
        "coldbox" : "x" // latest version from ForgeBox
        "slug" : "version", // a specific version from ForgeBox
        "slug" : "local filepath", //disallowed from forgebox registration
        "slug" : "URL",
        "slug" : "Git/svn endpoint"
    },
    // only needed on development
    "DevDependencies" : {
        // Same as above, but not installed in production
    },
    // Tracks install locations so uninstall can work.
    "installPaths" : {
        "coldbox" : "coldbox" // relative to package root (no leading slash)
        "feeds" : "modules/feeds", // relative to package root (no leading slash)
        "Name" : "C:\\foo\\bar" // Outside root, so full path
    },
    // array of strings of files to ignore when installing the package similar to .gitignore pattern spec 
    "ignore" : [ "logs/*", "readme.md" ]
    // testbox integration
    "testbox" : {
        // the uri location of the test runner for is app or several with slug names
        "runner" : [
            { "cf9"   : "http://cf9cboxdev.jfetmac/coldbox/testing/runner.cfm" },
            { "railo" : "http://railocboxdev.jfetmac/coldbox/testing/runner.cfm" }
        ],
        "runner" : ""
        "Labels" : [],
        "Reporter" : "",
        "ReporterResults" : "/test/results"
        "Bundles" : [ "test.specs" ]
        "Directory" : { mapping : "test.specs", recurse: true }, 
        // directories or files to watch for changes, if they change, then tests execute
        "Watchers" : [ "/model" ] ,
        // after tests run we can do a notification report summary
        "Notify" : { 
            "Emails" : [],
            "Growl" : address,
            // URL to hit with test report
            "URL" : ""
        }
    }
}
```
