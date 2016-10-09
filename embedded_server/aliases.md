# Aliases

CommandBox allows you to create web aliases for the web server that are similar to virtual directories.  The alias path is relative to the web root, but can point to any folder on the hard drive.  Aliases can be used for static or CFM files.  

To configure aliases for your server, create an object under `web` called `alises`. The keys are the web-accessable virtual paths and the corresponding values are the relative or absolute path to the folder the alias points to.

Here's what your `server.json` might look like.
```
{
  "web" : {
    "aliases" : {
      "/foo" : "../bar",
      "/js" : "C:\static\shared\javascript"
    }
  }
}
```

Here's how to create aliases from the `server set` command:
```
server set web.aliases./foo = bar
```

> **info** On Adobe ColdFusion servers, .cfm files will be run automatically from inside an aliases directory.  On Railo and Lucee servers, you'll need to create a CF mapping that maps the alias name and path for .cfm files to work.

