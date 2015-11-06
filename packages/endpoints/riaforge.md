# RIAForge Endpoint

CommandBox can install projects from the popular site RIAForge.org. You can find projects via the web site and copy the URL slug for a given project to use in your installation.  

For example, if the URL to a given project is `http://javaloader.riaforge.org/`, the slug you'll want to use would be `javaloader`.


## Installation

To install a project from RIAForge, use the slug from the website's URL like so:

```bash
install riaforge:iwantmylastfm
```

This will  create a folder in your installation directory named after the project containing all the files in the zip.  

> **Info** Note this endpoint will only work for RIAForge projects who's download URL points to a zip file.

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
