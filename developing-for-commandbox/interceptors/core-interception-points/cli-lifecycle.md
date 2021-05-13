# CLI Lifecycle

## onCLIStart

Announced when shell first starts, but before any commands are run or output has been flushed to the console.

**interceptData**

* `shellType` - The string `interactive` if starting the interactive shell, or `command` if running a one-off command and exiting
* `args` - An array of arguments provided from the OS when `box` was executed.
* `banner` - A string containing the CommandBox banner text that displays when in interactive mode.

This fires every time the `reload` command runs and a fresh shell is created.

## onCLIExit

Announced right before the shell exits and control is returned back to the OS. This fires every time the `reload` command runs right before the shell is destroyed and re-created.

## onSystemSettingExpansion

Announced every time a system setting in the format of `${something}` is expanded. Any interceptor listening can provide an expanded value.

**interceptData**

* **setting** - The name of the setting with the default value removed.
* **defaultValue** - The text after the final `:` or an empty string if there is no default provided
* **resolved** - Set this to `true` if your interceptor expands the value and return `true` from your interceptor method so CommandBox stops the interceptor chain.
* **context** - A struct of values if this system setting is being expanded in the context of a JSON file.

