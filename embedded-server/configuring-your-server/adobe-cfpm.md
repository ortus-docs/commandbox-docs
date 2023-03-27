# Adobe cfpm

Adobe ColdFusion 2021 and later have a built in command in their server home called `cfpm` (ColdFusion Package Manager) used to install modules into the core of the ACF engine. &#x20;

CommandBox provides a passthrough command called `cfpm` which will locate the Adobe server and invoke the correct cfpm.bat or cfpm.sh script, passing along whatever arguments you've typed.

```bash
CommandBox> cfpm install orm,debugger,pdf
```

The CommandBox `cfpm` command will use the current working directory of the shell, or intercept data, if run from inside of a package script or server script to figure out which server to apply to.  It will also ensure the `JAVA_HOME` env var is set for the process.

If you need to force `cfpm` to act upon a specfific CommandBox server, set the following env var before running it:

```bash
set CFPM_SERVER=my-CommandBox-server-name
cfpm install orm
```
