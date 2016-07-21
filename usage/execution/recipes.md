# Recipes

If you want to automate several commands from your native shell, it will
be faster to use our **[recipe](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/recipe.html)** command that allows you to run
several CommandBox commands at once. This will allow you to only load
the CommandBox engine once for all those commands, but still be dumped
back at your native prompt when done.  Recipes can also just be useful for a series of commands you run on a regular basis.

Read more about the
**recipe** command in our [Command API docs](http://apidocs.ortussolutions.com/commandbox/current/index.html?commandbox/system/modules/system-commands/commands/recipe.html).

## Ingredients

Think of a recipe as a simple batch file for Windows or a shell script for Unix.  It's just a text file where you place one command on each line and they are executed in order.  Enter the commands exactly as you would from the interactive shell.  

Technically a recipe can have any file extension, but the default recommendation is `.boxr` which stands for "box recipe".  Lines that start with a # will be ignored as comments.

**buildSite.boxr**
```bash
# Start with an empty folder
rm mySite --recurse --force
mkdir mySite
cd mySite

# Initialize this folder as a package
init name=mySite version=1.0.0 slug=mySlug

# Scaffold out a site and a handler
coldbox create app mySite
coldbox create handler myHandler index

# Add some required package
install coldbox
install cbmessagebox,cbstorages,cbvalidation

# Set the default port
package set defaultPort=8081

# Start up the embedded server
start

```

##Get Cooking

Execute your recipe with the `recipe` command, giving it the path to the recipe file.

```bash
recipe buildSite.boxr
```

If any commands in the recipe stop and ask for input, the recipe will pause until you supply that input.  All commands that have confirmations, etc should have a `--force` flag for this purpose so you can run them headlessly without requiring your input.  See the `rm` command above for an example.

## Spice It Up

You can also bind the recipe with arguments that will be replaced inside of your recipe at run time. 
Pass any arguments as additional parameters to the `recipe` command and they will be passed along to the commands in your recipe.

### Named arguments
If you use named arguments to the recipe command, they will be accessible inside the recipe as $arg1Name, $arg2Name, etc. 

Consider the following recipe:

**notifyWinner.boxr**
```bash
echo "Hello there, $name\n You've won a $prize!"
```

You would call it like so:
```bash
recipe recipeFile=notifyWinner.boxr name=Luis prize="NEW CAR"
```

Output:
```bash
Hello there, Luis
You've won a NEW CAR!
```

Note, all parameters to the `recipe` command needed to be named, including the `recipeFile`.


### Positional Parameters

Now let's look at the same recipe set up to receive positional parameters.

```bash
echo "Hello there, $1\n You've won a $1!"
```

You would call it like so:
```bash
recipe notifyWinner.boxr Luis "NEW CAR"
```

Output:
```bash
Hello there, Luis
You've won a NEW CAR!
```

### Escaping Parameters

When using an arg as a parameter to a command in a recipe, make sure you wrap the variable in quotes if it might contain spaces.  Otherwise you will get syntax errors.

```bash
rm "$arg1"
```

In this recipe `$arg1` is replaced with the exact value so we wrap it in quotes in case it contain spaces

### Missing Args

If an argument is not bound, no error will be thrown, and the name of the argument will be left in the command.  There is currently no way to default the value of an arg.  Stay tuned for task runners which will give you much more control over custom scripts.

## Is there an echo in here?

You can use `echo on` and `echo off` in recipes to control whether the commands output to the console as they are executed.  This can be useful for debugging or confirming the success of commands with no output.  `Echo` is on by default.

Note, `echo off` doesn't suppress the output of the commands, just the printing of the command and its arguments prior to execution.  This does not use the actual `echo` command and is a feature that only applies during the execution of recipes.

```bash
# Now you see me
echo on
version

# Now you don't
echo off
version
```

Output: 

```bash
version
CommandBox 1.2.3.00000
echo off
CommandBox 1.2.3.00000
```
















