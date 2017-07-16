# Task Runners

CommandBox allows you to automate common jobs that you want to run via Task Runners.  Tasks are for when you want something more flexible than a recipe or just a simple .cfm execution.  Tasks are analogous to other build tools like Ant, except you get to use CFML to build them instead of XML!  Task runners are the next generation of automation for CFML developers.  Now you don't need to learn another tool to improve your workflow.  You can automate directly in CFML!

A task is defined as a CFC and can have one or more "targets", which are declared as public methods on the CFC.  This gives your task a much more well-defined API than a simple .cfm execution which includes proper arguments, base methods, print helpers, and portability but with very little boilerplate.  Task runners operate very similar to custom commands, but instead of needing to be distributed inside a module, they are self contained in the CFC and can be dropped in any folder.  

## Build your first task

Let's look at how easy it is to write your first task:
```
CommandBox> touch task.cfc --open
```
Now, place the following code in your file:
```javascript
component {
  function run() {
    print.greenLine( 'This is my first task!' );
  }
}
```
Aaaaand, now we run it!
```
CommandBox> task run
This is my first task!
```
That's it!  The code in your `run()` method will be executed and has access to all the goodies that custom commands get like the `print` helper for easy ANSI formatting.  Check out task run help for additional information on how to call a task CFC of another name, how to invoke another target method, and how to pass parameters to your tasks.  