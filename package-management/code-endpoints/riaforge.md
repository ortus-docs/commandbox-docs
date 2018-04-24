# RIAForge

CommandBox can install projects from the popular site RIAForge.org. You can find projects via the web site and copy the URL slug for a given project to use in your installation.

For example, if the URL to a given project is `http://javaloader.riaforge.org/`, the slug you'll want to use would be `javaloader`.

## Installation

To install a project from RIAForge, use the slug from the website's URL like so:

```bash
install riaforge:iwantmylastfm
```

This will create a folder in your installation directory named after the project containing all the files in the zip.

> **Info** Note this endpoint will only work for RIAForge projects who's download URL points to a zip file.

## Package Metadata

Packages installed from the RIAForge endpoint don't have any way to get new version information. They will always show as outdated using the `outdated` or `update` commands and their downloads will not get stored in the artifact cache.

If the package has a `box.json`, its version information will be used, and any dependencies will be installed as well.

## In box.json

You can specify packages from the CFLib endpoint as dependencies in your `box.json` in this format.

```javascript
{
    "dependencies" : {
        "iwantmylastfm" : "riaforge:iwantmylastfm"
        "javaloader" : "riaforge:javaloader"
    }
}
```

