# Recipes

If you want to automate several commands from your native shell, it will
be faster to use our **[recipe](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/recipe.html)** command that allows you to run
several CommandBox commands at once. This will allow you to only load
the CommandBox engine once for all those commands, but still be dumped
back at your native prompt when done.  Recipes can also just be useful for a series of commands you run on a regular basis.

Read more about the
**recipe** command in our [Command API docs](http://apidocs.ortussolutions.com/commandbox/1.0.0/index.html?commandbox/system/commands/recipe.html).

Think of a recipe as a batch file for Windows or a shell script for Unix.  

```bash
C:\>box recipe myRecipe.boxr
```
