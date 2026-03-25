---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/Bw6M3PI3e5HgLVcKZz0G/usage/command-help
---

# Command Help

Help is integrated at every level in CommandBox. You can help global help, namespace help, or command help at any time.

## Global Help

To get an overall list of all the commands you have available to run, simply type `help` at the shell.

<figure><img src="../.gitbook/assets/global_help.png" alt="CommandBox Global Help"><figcaption><p>Global Help</p></figcaption></figure>

## Namespace Help

Next, drill down and get help on a specific namespace like `server`.

<figure><img src="../.gitbook/assets/namespace_help.png" alt="CommandBox Namespace Help"><figcaption><p>Namespace Help</p></figcaption></figure>

## Command Help

And finally, get help on a single command such as `server stop`. We can see the command is also aliased as just `stop` as well as all the possible parameters and their types along with a few sample ways to call the command.

<figure><img src="../.gitbook/assets/command_help.png" alt="Server Stop help information"><figcaption><p>Command Help</p></figcaption></figure>

## HTML Command API Docs

For a full list of all the commands that ship with CommandBox as well as all their paramaters and samples, please visit our [Command API docs](http://apidocs.ortussolutions.com/commandbox/current) which are auto-generated each build. This is the same information available to you via the `help` command, but in a searchable format you can browse outside of the CLI.

* [http://apidocs.ortussolutions.com/commandbox/current](http://apidocs.ortussolutions.com/commandbox/current)

## System Logs

Sometimes, you need to view the CommandBox log file. Maybe it is to debug a command you are writing or to [submit a crash report](https://ortussolutions.atlassian.net/secure/RapidBoard.jspa?rapidView=24\&projectKey=COMMANDBOX). The `system-log` command outputs the path to the CommandBox log file. You can use it creatively by piping its output in to other commands:

```bash
CommandBox> system-log | open
CommandBox> system-log | cat
CommandBox> system-log | tail
```
