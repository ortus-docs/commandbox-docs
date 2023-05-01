# watch Command

There are some specific commands that make use of the Watcher library in CommandBox such as `testbox watch` and `coldbox watch-reinit`. However, there is also a generic watch command that will run any arbitrary command of your choosing when a path matching your file globbing pattern is added/updated/deleted.

```bash
watch paths=*.json command="echo 'config file updated!'"
```

The following environment variables will be available to your command

* **watcher\_added** - A JSON array of relative paths added in the watch directory
* **watcher\_removed** - A JSON array of relative paths removed in the watch directory
* **watcher\_changed** - A JSON array of relative paths changed in the watch directory

Since escaping meta characters can get tricky with nested strings, you can declare the command as an environment variable and then just reference it like so:

```bash
set command = "echo 'You added \${item}!'"
watch command="foreach '\${watcher_added}' \${command}" --verbose
```

That example runs the `foreach` command over each item in the `watcher_added` array and then runs an `echo` statement to output the name of each added path.

Note that the "paths" here work more like `.gitignore` entries and less like bash paths. Specifically:

* A path with a leading slash (or backslash), will be evaluated relative to the current working directory. E.g. `watch /foo` will only watch files in the directory at `./foo`, but not in directories like `./bar/foo`.
* A path without a leading slash (or backslash) will be applied as a glob filter to _all files_ within the current working directory. E.g. `watch foo` will result in the entire working directory being watched, but only files matching the glob `**foo` will be processed.

If your watcher seems slow, unresponsive, or is failing to notice some file change events, it is likely that you have it watching too many files. Try specifying more specific paths to the files you want to process, and use leading slashes in your arguments to avoid watching all files in the current working directory.

### Parameters

* `paths` - Comma-delimited list of file globbing paths to watch relative to the working directory, defaults to \*\*
* `excludePaths` - Comma-delimited list of file globbing paths to exclude relative to the working directory.
* `command` - The command to run when the watcher fires
* `delay` - How may milliseconds to wait before polling for changes, defaults to 500 ms
* `directory` - Working directory to start watcher in
* `verbose` - Output details about the files that changed
