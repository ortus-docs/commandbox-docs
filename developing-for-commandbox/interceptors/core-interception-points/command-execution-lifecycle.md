# Command Execution Lifecycle

## preCommand

Announced before the execution of a command. This fires after all command parameters have been evaluated, including expressions. If piping the output of one command into another in a command chain, this will fire twice-- once for each command in the chain.

**interceptData**

* `commandInfo` - A struct containing the following keys about the command to execute
  * `commandString` - A string representing the command name
  * `commandReference` - The instantiated Command CFC
  * `parameters` - An array of un-parsed parameter tokens typed in the CLI
  * `closestHelpCommand` - The CFC path to the most-applicable help command. Used to generate namespace help.
* `parameterInfo` - A struct containing the following keys about the processed parameters for the command execution
  * `positionalParameters` - An array of parameter values
  * `namedParameters` - A struct of name/value pairs.  The named parameters are always what is passed to the command's `run()` method.
  * `flags` - A struct of flags that were passed in.

## preCommandParamProcess

**preCommand** fires after expression, system setting, and colon param expansions have already occurred. **preCommandParamProcess** will fire prior to that so you can affect the system prior to the time the system settings are expanded in parameter values.

* `commandInfo` - Same as above
* `parameterInfo` - Same as above

## postCommand

Announced immediately after command execution is complete. If more than one command is piped together in a command chain, this is announced after each command in the chain.

**interceptData**

* `commandInfo` - Same as `preCommand` above
* `parameterInfo` - Same as `preCommand` above
* `results` - A string that represents any output from the command that hasn't already been flushed to the console.

## prePrompt

Announced prior to drawing the prompt in the interactive shell.  This interception point can be used to customize the text of the prompt by modifying the `prompt` variable in intercept data which is an ANSI-formatted string to be output before the cursor.

**interceptData**

* `prompt` - An ANSI-formatted string containing the prompt text.  Replacing this value will override the prompt.

## preProcessLine

Pre and post command fire before and after each command, but that means they fire twice for something like:

```text
echo `package show name`
```

`preProcessLine` will fire only once for the above command after the user hits enter but before anything is processed.

**interceptData**

* `line` - A string representing the line typed into the shell.  Changing the contents of this string will override what actually gets executed.

## postProcessLine

Pre and post command fire before and after each command, but that means they fire twice for something like:

```text
echo `package show name`
```

`postProcessLine` will fire only once for the above command after the entire line has been executed.  Any output is already sent to the console by the time this interception point fires.

**interceptData**

* `line` - A string representing the line that was just executed in the shell. 

