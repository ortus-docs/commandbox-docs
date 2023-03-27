# Interactive Jobs

Sometimes you have a task that will run for a while or just produce a lot of logging output as it executes to let the user know what's going on. it is very easy to keep the user up to date with the print helper like so:

```
print.greenLine( 'Step 57 complete!' );
```

This can create a lot of output however which can look a little messy. Plus if you have a task that runs another task or command, all the output from each steps gets mixed together. This is where Interactive Jobs help you harness the power of redrawing the console over and over to nicely format what steps have completed and have logging for each step that goes away once the step is complete.

Interactive Jobs are best if you have a small enough number of jobs that they can all fit on one screen. Since the output of the jobs is updated in real time, it doesn't scroll past the bottom of the terminal and CommandBox will just truncate any extra text.

## Job DSL

We have a nice DSL you can call that signals the start and end of each job. Every job has a name, zero or more log messages, and a final status of whether it was successful. The ~~`job`~~ variable is automatically available for custom commands and Task Runners.

```javascript
job.start( 'This is my job to run' );
  sleep( 2000 );
job.complete();
```

The log messages will show up in the order they appear, but once you complete the job, the log messages are hidden and only a single, green line shows to represented the completed job regardless of how many steps it had in the meantime. Do not output normal text with the `print` helper if possible. Once you've started the job, use the `job.addLog()` calls.

The final output of the above code would be this green text:

![](<../.gitbook/assets/image (1) (1).png>)

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

By default, a job will only show the last 5 log lines to keep things tidy. Configure this when you start the job.

```javascript
job.start( name='My job', lineSize=10 );
```

## Ending the Job

All good things must come to an end. use the `complete()` method to show that your job has finished successfully. use the `error()` method to show your job has ended but with issues.

```javascript
job.start( 'I have a good feeling about this' );
if( warmFuzzy ) {
  job.complete();
} else {
  job.error( 'I''ve lost that loving feeling.' );
}
```

The `job.error()` method can take an optional message to describe what happened which will remain on the screen. If a Task Runner has an unhandled error or the user interrupts execution with Ctrl-C, CommandBox will end the job for you as an error. The exception message will be passed to the `job.error()` call for you so the user can see what happened.

### Dump Log Messages

If you want to have a verbose mode in your task that dumps out all the log messages at the end you can do that by passing `dumpLog` as true in your `job.complete()` or `job.error()` calls. This is great for debugging tasks that ran on a CI server. This dumps ALL logging lines regardless of what your `logSize` was set to. `logSize` is only used for the interactive output during execution to keep things clean.

```javascript
job.complete( dumpLog=true );
```

## Nesting Jobs

Ok, so here is where it really gets cool. Let's say you have a Task Runner that starts a server, which in turn installs a new Adobe CF engine. That's 3 jobs all nested inside of each other. You can call job.start() at any time, and the Interactive Job handler will keep a call stack of what's going on. Log messages always get added to the last job to be started. (Last In, First Out)

```javascript
job.start( 'Starting server' );
  job.addLog( 'This is the server name' );
  job.addWarnLog( 'Hey, don''t touch that dial' );

    job.start( 'Installing CF Engine first' );
      job.addLog( 'This was the version used' );
      job.addLog( 'Yeah, we''re done' );
    job.complete();

  job.addLog( 'Aaand, we''re back!.' );
  job.addErrorLog( 'I think we''re going to crash' );

job.error( 'Didn''t see that coming' );
```

The output of this is:

![](<../.gitbook/assets/image (8) (1) (1) (2) (2) (2) (2) (2) (2) (1).png>)

Here we have two nested jobs. The first two lines would be red representing the outer failed job and it's failure message. The 3rd indented line would be green representing the nested job that completed successfully.

And if we add `dumpLog` to each job, what do we get?

![](https://github.com/ortus-docs/commandbox-docs/tree/df981947c5780503203384f9de7118f57ee01ca5/.gitbook/assets/image%20\(4\).png)

## Other Considerations

Interactive jobs are fully compatible with progressable downloaders as well as the multi-select input control. Any progress bars will display at the bottom of the job output and disappear when complete.

If for some reason you need to call an external process that outputs text that you can't control and funnel through a job or you need to stop and ask the user a question with the `ask()` function, you can temporarily clear out output from all current jobs.

```javascript
job.clear();
```

All the output will come back the next time you call a job method.

Check if there is currently a job running like so:

```javascript
if( job.isActive() ) {
}
```

If there are several nested jobs currently running and something catastrophic happens and you just want to mark all the in progress ones as errors without necessarily knowing how many are still currently running you can do this:

```javascript
job.errorRemaining( 'That escalated quickly!' );
```

This can be handy to call from some high level error handling that can catch errors from tasks/commands/functions several layers deep. It's basically your escape hatch.
