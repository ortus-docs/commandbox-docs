# Test Runner

## Run unit test suite from the command line

There is a `testbox run` command available that will run your unit or integration tests from the command line. All you need is to have your server running \(it doesn't have to be a CommandBox server\).

Run your test suite like so \(modify the runner URL to match your site\).

```text
testbox run "http://localhost:8080/tests/runner.cfm"
```

This will hit your runner URL and output a CLI-formatted report of your test results. If your test suite failed, the command will also return a failing status code when being run from your native shell. This makes integrations with CI tools like Jenkins or Travis-CI very easy since a failing test will fail your build automatically.

### Default runner URL

You can also set up the default runner URL in your box.json and it will be used for you. Setting the URL is a one-time operation.

```text
package set testbox.runner="http://localhost:8080/tests/runner.cfm"
testbox run
```

You can also use a relative path and CommandBox will look up the host and port from your server settings.  

```text
package set testbox.runner="/tests/runner.cfm"
testbox run
```

The default runner URL of the `testbox run` command is  `/tests/runner.cfm` so there's actually no need to even configure it if you're using the default convention location for your runner.

### Additional Settings

There are a number of settings you can provide to the `testbox run` command to control exactly what tests run and how much output is included. Each of these can also be set in your box.json. Run this for more information.

```text
testbox run help
```

## Example Output

Here is an example of the non-verbose output.

```text
testbox run --noVerbose

Executing tests via http://127.0.0.1:55197/tests/runner.cfm?&recurse=true&reporter=json, please wait...
TestBox v2.5.0+107
---------------------------------------------------------------------------------
| Passed  | Failed  | Errored | Skipped | Time    | Bundles | Suites  | Specs   |
---------------------------------------------------------------------------------
| 8       | 0       | 0       | 0       | 112 ms  | 1       | 3       | 8       |
---------------------------------------------------------------------------------
```

