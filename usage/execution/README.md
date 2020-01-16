# Execution

There are two ways to run commands via CommandBox: inside the CommandBox interactive shell, or one-at-a-time commands from your native shell.

## Multiple Commands

If you open the interactive shell, you will see the CommandBox splash screen \(ASCII art\) and then you'll be presented with the `CommandBox>` prompt. You can enter as many commands as you wish in order and after each command is finished executing, you will be returned to the CommandBox prompt. If you have multiple commands you want to execute manually, this is the fastest method since CommandBox only loads once. This is also the only way to make use of features like [tab complete](../tab-completion.md) and [command history](https://github.com/ortus/commandbox-documentation/tree/c1beb7155f97414bba701c3e9999520d300f6465/usage/history.md).

This example show running the `box.exe` executable from a Windows DOS prompt, executing the [version](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/version.html), [pwd](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/pwd.html), and [echo](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/echo.html) commands, and then exiting back to DOS.

```bash
C:\>box.exe

   _____                                          _ ____            
  / ____|                                        | |  _ \           
 | |     ___  _ __ ___  _ __ ___   __ _ _ __   __| | |_) | _____  __
 | |    / _ \| '_ ` _ \| '_ ` _ \ / _` | '_ \ / _` |  _ < / _ \ \/ /
 | |___| (_) | | | | | | | | | | | (_| | | | | (_| | |_) | (_) >  < 
  \_____\___/|_| |_| |_|_| |_| |_|\__,_|_| |_|\__,_|____/ \___/_/\_\  v1.2.3.00000

Welcome to CommandBox!
Type "help" for help, or "help [command]" to be more specific.
CommandBox> version
CommandBox 1.2.3.00000
CommandBox> pwd
C:\
CommandBox> echo "Hello World!"
Hello World!
CommandBox> exit

C:\>
```

## One-Off Commands

You can also spin up CommandBox from your native shell to execute a single command inline. You can do this if you only have one command to run, or you want to automate a command from a Unix shell script or Windows batch file. This mode will not show the ASCII splash screen, but keep in mind it still loads CommandBox up and unloads it in the background. Any output from the command will be left on your screen, and you will be returned to your native OS prompt.

Here is an example of running the version command from a Windows DOS screen. Note, you'll need to either do this from the directory that holds the box executable, or add the executable to your default command path so it is found.

```bash
C:\>box version
CommandBox 1.2.3.00000

C:\>
```

The `box` text is calling the CommandBox binary, and the `version` bit is passed along to the CommandBox shell to execute once it loads.

## Debug Mode

You can also activate CommandBox in **debug** mode by passing the `-debug` flag in the command line. This will give you much more verbose information about the running CommandBox environment. This affects both the interactive shell or one-off commands

```bash
box version -debug
box server start -debug
```

## Output

Output from commands will be ANSI-formatted text which, by default, streams directly to the console. When in the interactive shell, you can capture the output of commands and manipulate it, search it, or write it to a file. Use a pipe \(`|`\) to pass the output of one command into another command as its first input. Output can be piped between more than one command. Use a right bracket \(`>`\) and double right bracket \(`>>`\) to redirect output to the file system.

### Search

Pipe output into the [**grep**](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/grep.html) command to apply a regex upon it. [**grep**](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/grep.html) will only emit lines matching the regex.

```bash
cat myLogFile.txt | grep "variable .* undefined"
```

### Pagination

Pipe output into the [**more**](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/more.html) command to output it line-by-line or page-by-page. Press the spacebar to advance one line at a time. Press the Enter key to advance one page at a time. Press ESC or “q” to abort output.

```bash
forgebox show | more
```

### Redirection

Redirect output into a file, overwriting if it exits like so:

```bash
dir > fileList.txt
```

[API Docs for fileWrite.](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/fileWrite.html)

Use the double arrows to append to an existing file.

```bash
echo "Step 3 complete" >> log.txt
```

[API Docs for fileAppend.](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/fileAppend.html)

### Tail files

You can pipe a large amount of text or a file name into the `tail` command to only output the few lines of the text/file. Adding the `--follow` flag when tailing a file will live-stream changes to the file to your console until you press Ctrl-C to stop.

```text
forgebox search luis | tail
system-log | tail lines=50
tail myLogFile.txt --follow
```

### Ad-hoc Java properties for the CLI

If you want to add ad-hoc Java Properties to the actual CLI process, you can set an environment variable in your OS called `BOX_JAVA_PROPS` in this format:

```bash
BOX_JAVA_PROPS="foo=bar;brad=wood"
```

That would create a property called `foo` and a property called `brad` with the values `bar` and `wood` respectively.  This environment variable works the same on all operating systems.

### Ad-hoc JVM args for the CLI

Similar to above, you may want to add ad-hoc JVM args to the java process that powers the CLI.  The steps differ per operating system.  For \*nix \(Linux, Mac\), set an environment variable called `BOX_JAVA_ARGS` in the environment that `box` will run in.

```bash
BOX_JAVA_ARGS="-Xms1024m -Xmx2048m -Dfoo=bar"
box
```

For Windows, create a file called `box.l4j.ini` in the same directory as the `box.exe` file and place a JVM arg on each line.  Escape any backslashes with an additional backslash like a properties file format.

{% code title="box.l4j.ini" %}
```bash
-Xms1024m
-Xmx2048m
-Dfoo=bar
```
{% endcode %}

Both of those examples would set the min/max heap size of the CLI process and also set a Java System Property called "foo" equal to "bar".  There is no effective difference between setting system properties this way as opposed to using `BOX_JAVA_PROPS` as shown in the previous section, but actual JVM `-X` settings must be set as described in this section.

## Noninteractive Mode

If you are using CommandBox in a continuous integration server such as Jenkins or Travis-CI, you may find that features like the progress bar which redraw the screen many times create hundreds of lines of output in the console log for your builds.  You can enable a non interactive mode that will bypass the output from interactive jobs and the download progress bar.

```text
config set nonInteractiveShell=true
```

If there is no `nonInteractiveShell` setting, CommandBox will automatically default it to true if there is an environment variable named `CI` present, which is standard for many build servers such as Travis-CI.

