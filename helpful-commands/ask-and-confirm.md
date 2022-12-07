# ask and confirm

Here are some fun commands for user interactivity in the shell.  You can use these as part of a recipe or a nice "one-liner".

### ask

The `ask` command is similar to the `ask()` method in Task Runners.  It requires an interactive terminal and will ask the user a question and return their answer.  It is meant to be changed with other commands.

```bash
set color=`ask "favorite Color? "`
echo "you said ${color}"
```

or with default values

```bash
ask question="Who is cool? " defaultResponse="Balbino!"
```

or with masked input

```bash
ask question="What is your password? " mask=*
```

Or fun stuff like this

```bash
ask "Secret phrase: " | assertEqual "mockingbird" || echo "access denied!" && exit 1
```

### confirm

The `confirm` command will ask the user a yes/no question and return a passing or failing exit code from the command based on the answer.

```bash
confirm "do you want to update your packages? " && update
```

Remember the `&&` operator will only execute the second command if the first command returns an exit code of `0`.
