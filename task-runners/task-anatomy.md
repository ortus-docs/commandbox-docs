# Task Runner Anatomy

In the previous section, we built our first task and executed it.  Let's take a look at how the simple conventions work for task runners.

## Default conventions
The `task run` command will look for a default task name of `task.cfc` in the current working directory. The default target (or method) is called `run`. Therefore, when you run
```
task run
```
you will execute the `run()` method of `./task.cfc` relative to your current working directory.

## Take you to task
A task is just a CFC file.  The task CFCs will extend the `commandbox.system.BaseTask` class, but it isn't necessary for your to extend it manually.  CommandBox will add in virtual inheritance for you.  The task CFCs can live wherever you want in your project (or outside of it!) and can have any name you wish.  Since tasks are not scanned and registered by CommandBox, but rather created on-the-fly by convention, you don't have to worry about more than one task in different projects with the same name.  That means, each of your projects can have a `workbench/build.cfc` task runner and they won't collide.  When you run the task, the correct CFC will be located based on your current working directory.  Therefore, tasks are not "global" like commands, but rather in the context of a given project or folder.



## Hit the target
You can have as many targets in your task as you wish, by simply declaring public methods.  A task can have no parameters or as many as you like.  Default argument values will be used as well.  It's just a method execution, but from the command line!