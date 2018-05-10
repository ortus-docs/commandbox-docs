# Interactive Jobs

Sometimes you have a task that will run for a while or just produce a lot of logging output as it executes to let the user know what's going on.  it is very easy to keep the user up to date with the print helper like so:

```text
print.greenLine( 'Step 57 complete!' );
```

This can create a lot of output however which can look a little messy.  Plus if you have a task that runs another task or command, all the output from each steps gets mixed together.  This is where Interactive Jobs help you harness the power of redrawing the console over and over to nicely format what steps have completed and have logging for each step that goes away once the step is complete.  

Interactive Jobs are best if you have a small enough number of jobs that they can all fit on one screen.  Since the output of the jobs is updated in real time, it doesn't scroll past the bottom of the terminal and CommandBox will just truncate any extra text. 

## Job DSL

We have a nice DSL you can call that signals the start and end of each job.  Every job has a name, zero or more log messages, and a final status of whether it was successful.  The ~~`job`~~ variable is automatically available for custom commands and Task Runners.

```javascript
job.start( 'This is my job to run' );
  sleep( 2000 );
job.complete();
```

The log messages will show up in the order they appear, but once you complete the job, the log messages are hidden and only a single, green line shows to represented the completed job regardless of how many steps it had in the meantime.  Do not output normal text with the `print` helper if possible.  Once you've started the job, use the `job.addLog()` calls.

## Log Messages

You can log the individual steps of your job for instant user feedback:

```javascript
job.start( 'This is my job to run' );
  sleep( 2000 );
  job.addLog( 'Still going...' );
  
  sleep( 2000 );
  job.addLog( 'Now we''re getting somewhere.' );
  
  sleep( 2000 );
  job.addLog( 'Almost done!' );
  
  sleep( 2000 );
job.complete();
```

Feel free to add ANSI formatting to your log messages, but we have some convenience method for you already.

```javascript
job.addLog( 'I will print white text' );
job.addSuccessLog ( ' I will print green text' );
job.addWarnLog( ' I will print yellow text' );
job.addErrorLog( ' I will print red text' );
```

By default, a job will only show the last 5 log lines to keep things tidy.  Configure this when you start the job.

```javascript
job.start( name='My job', lineSize=10 );
```

## Ending the Job

All good things must come to an end.  use the `complete()` method to show that your job has finished successfully.  use the `error()` method to show your job has ended but with issues.  

```javascript
job.start( 'I have a good feeling about this' );if( warmFuzzy ) {  job.complete();} else {  job.error( 'I''ve lost that loving feeling.' );}
```

The `job.error()` method can take an optional message to describe what happened which will remain on the screen. If a Task Runner has an unhandled error or the user interrupts execution with Ctrl-C, CommandBox will end the job for you as an error.  The exception message will be passed to the `job.error()` call for you so the user can see what happened.

### Dump Log Messages

If you want to have a verbose mode in your task that dumps out all the log messages at the end you can do that by passing `dumpLog` as true in your `job.complete()` or `job.error()` calls.  This is great for debugging tasks that ran on a CI server. This dumps ALL logging lines regardless of what your `logSize` was set to.  `logSize` is only used for the interactive output during execution to keep things clean.

```text
job.complete( dumpLog=true );
```

