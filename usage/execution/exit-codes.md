# Exit Codes

CommandBox follows the standard system of exit codes. In Bash or DOS, every process has an exit code that is available to the shell after execution is complete. The are as follows:

* **0** - Success
* **Any other number** - Failure

This slightly counterintuitive to CFML developers since we are used to positive numbers as being truthy and zero as being falsely, but it is the industry standard for shells. It makes more sense if you think of it in terms of what Windows calls it-- `%errorlevel%`. if the error level is 0 there was no error!

## Command Exit Codes

Every command that executes has an exit code. Usually the exit code is 0 if the command ran as expected. If an error of any kind was encountered, then the exit code will be a non-zero number. Often times 1. You can easily see this if you install the [CommandBox Bullet Train](https://forgebox.io/view/commandbox-bullet-train) module as it shows you the exit code of the last command to run. Commands such as `testbox run` will return a failing exit code if the tests being run didn't all pass.

You can access the last exit code from CommandBox in a [System Setting](../system-settings.md) called `exitCode`.

```bash
> !git status
fatal: not a git repository (or any of the parent directories): .git
Command returned failing exit code [128]
​
> echo ${exitCode}
128
```

## Shell Exit Code

The CommandBox shell also keeps track of exit code of the last command. When the shell exits, it will report that last exit code to the OS. When running a one-off command from your native shell, the exit code of that command will be passed straight through to your native shell. This means that running something like

```bash
$> box testbox run
```

from a Travis-CI build will automatically fail the build if the tests don't pass.

## Manual Exit Code

You can manually return an exit code from the shell passing the desired number to the `exit` command and the native OS will receive that code from the `box` binary.

```bash
exit 123
```

## Command Chaining

Similar to bash, CommandBox allows you to chain multiple commands together on the same line and make them conditional on whether the previous command was successful or not.

### &&

You can use `&&` to run the second command only if the previous one **succeeded**.

```bash
mkdir foo && cd foo
```

### ||

You can use `||` to run the second command only if the previous one **failed**.

```bash
mkdir foo || echo "I couldn't create the directory"
```

### ;

You can use a single semi colon (`;`) to separate commands and each command will run regardless of the success or failure of the previous command.

```bash
mkdir foo; echo "I always run"
```

## Assertions

With the above building blocks, we can get clever to create simple conditionals to only run commands if a condition is met. Or these can simply be used to cause recipes to stop execution or to fail builds based on a condition. The following commands output nothing, but they return an appropriate exit code based on their inputs.

### pathExists

Returns a passing (0) or failing (1) exit code whether the path exists. In this example, we only run the package show command if the `box.json` file exists.

```bash
pathExists box.json && package show
```

You can specify if the path needs to be a file or a folder.

```bash
pathExists --file server.json && server show
pathExists --directory foo || mkdir foo
```

### assertTrue

Returns a passing (0) or failing (1) exit code whether truthy parameter passed. Truthy values are "yes", "true" and positive integers. All other values are considered falsy

```bash
assertTrue `package show private` && run-script foo
assertTrue ${ENABLE_DOOM} && run-doom
assertTrue `#fileExists foo.txt` && echo "it's there!"
```

### assertEqual

Returns a passing (0) or failing (1) exit code whether both parameters match. Comparison is case insensitive.

```bash
assertEqual `package show name` "My Package" || package set name="My Package"
assertEqual ${ENVIRONMENT} production && install --production
```
