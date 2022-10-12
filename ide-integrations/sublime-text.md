# Sublime Text

A plugin to run your [Commandbox](https://www.ortussolutions.com/products/commandbox) tasks from within Sublime plus some handy [snippets](sublime-text.md#snippets) too.

## Quickstart

1. Install Via [Package Control](https://packagecontrol.io) `Commandbox`

## Installation

### via PackageControl

If you have [PackageControl](http://wbond.net/sublime\_packages/package\_control) installed, you can use it to install the package.

Just type `cmd-shift-p`/`ctrl-shift-p` to bring up the command palette and pick `Package Control: Install Package` from the dropdown, search and select the `CommandBox` package there and you're all set.

### Manually

You can clone the repo in your `/Packages` (_Preferences -> Browse Packages..._) folder and start using/hacking it.

```
cd ~/path/to/Packages
git clone git://github.com/Ortus-Solutions/sublime-commandbox.git Commandbox
```

### Troubleshooting

If you are having trouble running the plugin in **Mac OSX** it's possible that your path isn't being reported by your shell. In which case give the plugin [SublimeFixMacPath](https://github.com/int3h/SublimeFixMacPath) a try. It may resolve our issue.

If you still can't get it to run properly, first make sure your Commandbox tasks run from a terminal (i.e. outside of sublime) and if so then submit an [issue](https://github.com/Ortus-Solutions/sublime-commandbox/issues).

## Usage

### Running a Commandbox Command

You may select pre-configured commands from `Menu -> Commandbox`, including the ability enter a custom command.

#### Keyboard Shortcuts:

* `Ctrl+Shift+B` - Run Commandbox Command: A prompt input will open for you to enter your command
* `Ctrl+Shift+T` - Start the Embedded Server: Your Commandbox `cwd` is always the root of your Sublime Project so any `box.json` configuration will be honored
* `Ctrl+Shift+P` - Stop the Embedded Server
* `Alt+P` - Show the Commandbox Output Panel ( also available in the `View -> Commandbox` menu )
* `Alt+[Command|Windows]+P` - Hide the Commandbox Output Panel ( also available in the `View -> Commandbox` menu )

## Settings

The file `Commandbox.sublime-settings` is used for configuration, you can change your user settings in `Preferences -> Package Settings -> Commandbox -> Settings - User`.

The defaults are:

```javascript
{
    // Override your environment PATH
    "exec_args": {},
    // Use a new tab when showing the results. If it's false it'll use a panel.
    "results_in_new_tab": false,
    // Defines the delay used to autoclose the panel or tab that holds the box results.
    // If false (or 0) it will remain open.
    "results_autoclose_timeout_in_milliseconds": 0,
    // If true it will open the output panel when running Commandbox(silent) only if the task failed
    "show_silent_errors": true,
    // Create the file 'sublime-commandbox.log' to report errors
    "log_errors": true,
    // Syntax file for highlighting the box results
    // Set to false if you don't want any colors (you may need to restart Sublime)
    "syntax": "Packages/Commandbox/syntax/CommandboxResults.tmLanguage",
    // Read from stdout and stderr without blocking (both at the same time)
    "nonblocking": true,
    // Add a custom flag to a particular box command. Format: { "task_name": "flags" }
    // For example: { "concat": "--silent" }
    "flags": {},
    // If `false` the package will run even if no `box.json` is found on the root folders currently open.
    "check_for_boxjson": true,
}
```

#### exec\_args

You may override your `PATH` environment variable as follows:

```javascript
{
    "exec_args": {
        "path": "/bin:/usr/bin:/usr/local/bin"
    }
}
```

**box installed locally**

If box is installed locally in the project, you have to specify the path to the box executable. Threfore, adjust the path to `/bin:/usr/bin:/usr/local/bin:node_modules/.bin`

#### results\_in\_new\_tab

If set to `true`, a new tab will be used instead of a panel to output the results.

#### results\_autoclose\_timeout\_in\_milliseconds

Defines the delay used to autoclose the panel or tab that holds the Commandbox results.

#### show\_silent\_errors

If true it will open the output panel when running [`Commandbox (silent)`](sublime-text.md#running-a-box-task) only if the task failed

#### log\_errors

Toggles the creation of `sublime-commandbox.log` if any error occurs.

#### syntax

Syntax file for highlighting the box results. You can pick it from from the command panel as `Set Syntax: Commandbox results`.

Set the setting to `false` if you don't want any colors (you may need to restart Sublime if you're removing the syntax).

#### nonblocking

When enabled, the package will read the streams from the task process using two threads, one for `stdout` and another for `stderr`. This allows all the output to be piped to Sublime live without having to wait for the task to finish.

If set to `false`, it will read first from `stdout` and then from `stderr`.

#### Bind Your Own Keyboard Shortcuts

You can use a shortcut for running a specific task like this:

```javascript
{ "keys": ["KEYS"], "command": "commandbox", "args": { "task_name": "watch" } }
```
