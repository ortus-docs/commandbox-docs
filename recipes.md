# Recipes

If you want to automate several commands from your native shell, it will
be faster to use our **[recipe](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/recipe.html)** command that allows you to run
several CommandBox commands at once. This will allow you to only load
the CommandBox engine once for all those commands, but still be dumped
back at your native prompt when done.  Recipes can also just be useful for a series of commands you run on a regular basis.

Read more about the
**recipe** command in our [Command API docs](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/recipe.html).

## Ingredients

Think of a recipe as a simple batch file for Windows or a shell script for Unix.  It's just a text file where you place one command on each line and they are executed in order.  Enter the commands exactly as you would from the interactive shell.  

Technically a recipe can have any file extension, but the default recommendation is `.boxr` which stands for "box recipe".  Lines that start with a # will be ignored as comments.

**BuildSite.boxr**
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
install cbmessagebox,cbstorages,cbvaidation

# Set the default port
package set defaultPort=8081

# Start up the embedded server
start

```

#Get Cooking

Execute your recipe with the `recipe` command, giving it the path to the recipe file.

```bash
CommandBox>recipe BuildSite.boxr
```

If any commands in the recipe stop and ask for input, the recipe will pause until you supply that input.  All commands that have confirmations, etc should have a `--force` flag for this purpose so you can run them headlessly without requireing your input.  See the `rm` command above for an example.












