# Environment Variables

CommandBox allows you to access Java System Properties from the CLI and environment variables from your operating system via the `${name_here}` mechanism. But CommandBox also gives you the ability to create variables of your own directly in the shell. The scope \(life\) of these environment variables depends on how and where they are declared.

## Shell Environment Variables \(Global\)

Every shell instance has its own set of environment variables you can set and read. These live for the duration of the shell and have the same lifespan as Java system properties in the CLI. There is an `env` namespace of commands for dealing with environment variables.

To set a new variable, you can run this:

```bash
env set foo=bar

# or just
set foo=bar
```

You can view the contents of that variable from anywhere in the same shell session like so:

```bash
env show foo

# or provide a default value
env show foo myDefault

# Or via System Setting expansions
echo ${foo:myDefault}
```

Clear out an environment variable like so:

```bash
env clear foo
```

## Per-Command Environment Variables

In addition to the global shell environment, each executing command receives its own set of environment variables that go away when the command finishes executing. Unlike Bash, the parent variables are not copied into each command, but a hierarchy is maintained and when a variable is not found, the parent command is checked and so on until the global shell variables. After that, the Java system properties and then OS's actual environment is checked.

Here is a contrived example of an `echo` command that run a sub command as part of a command expression \(backtick expansion\). The sub command sets an environment variable and then outputs it.

```bash
echo `set myVar=cheese && echo \${myVar}`
env show myVar myDefault
```

The first line will output the text "Cheese" but the second line will output the default value of "myDefaul" since the variable only existed in the inner context of the expression and isn't visible to the outer shell.

## Recipes

Recipes behave like a subshell and any variables set in a recipe will live for the duration of that recipe but will be gone once the recipe exits.

## Viewing Variables

To view the environment variable for the current command context, run:

```bash
> env set color=red
> env set number=2
> env show
{
    "number":"2",
    "color":"red"
}
```

The output is a JSON serialized struct, which makes it suitable for programmatically accessing.

```bash
> env show | foreach "echo 'The var \${item} is set to \${value}'"
The var number is set to 2
The var color is set to red
```

There is a command to help you debug what variables are set in each command context as well as the global shell. Here we pipe some commands into a recipe for execution. The context for the `env debug` command has no variables, the `recipe` command has a variable called `inRecipe` and the global shell as the 2 variables from the example above.

```bash
> echo "set inrecipe=true; env debug" | recipe
[
    {
        "environment":{},
        "context":"env debug"
    },
    {
        "environment":{
            "inRecipe":"true"
        },
        "context":"recipe"
    },
    {
        "environment":{
            "number":"2",
            "color":"red"
        },
        "context":"Global Shell"
    }
]
```

## Native Binaries

Any environment variables you set in the CommandBox shell will be available to the native process that your OS binary runs in. Here's a Windows and \*nix example of setting an env var in CommandBox and then using it from the native shell.

```bash
set name=brad
!echo %name%
```

```bash
set name=brad
!echo $name
```

## Passing Environment Variables to Servers

Any ComandBox environment variables present in the shell will automatically be passed to the environment of the server process. This means, given an example like this:

```bash
set foo=bar
server start
```

The CFML code running that server process will be able to "see" the `foo` environment variable.

