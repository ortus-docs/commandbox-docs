# Test Watcher

This is an extension of the `testbox run` command but will watch the files in a directory and run the default TestBox suite on any file change.

```text
testbox watch
```

In order for this command to work, you need to have started your server and configured the URL to the test runner in your `box.json`.

```text
package set testbox.runner=http://localhost:8080/tests/runner.cfm
server start
testbox watch
```

You can also control what files to watch.

```text
testbox watch **.cfc
```

If you need more control over what tests run and their output, you can set additional options in your `box.json` which will be picked up automatically by `testbox run` when it fires.

```text
package set testbox.verbose=false
package set testbox.labels=foo
package set testbox.testSuites=bar
package set testbox.watchDelay=1000
package set testbox.watchPaths=/models/**.cfc
```

This command will run in the foreground until you stop it. When you are ready to shut down the watcher, press `Ctrl+C`.

