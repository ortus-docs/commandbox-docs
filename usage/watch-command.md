# watch command

There are some specific commands that make use of the Watcher library in CommandBox such as `testbox watch` and `coldbox watch-reinit`.  However, there is also a generic watch command that will run any arbitrary command of your choosing when a path matching your file globbing pattern is added/updated/deleted. 

```bash
watch *.json "echo 'config file updated!'"
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

