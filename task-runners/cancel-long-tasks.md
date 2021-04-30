# Cancel Long Tasks

In CommandBox, a user can cancel a command or task by pressing Ctrl-C. This fires the interrupt handler in the terminal which calls `Thread.interrupt()` on the Java thread running in CLI. Java's interrupt doesn't kill a thread dead though, it politely asks it to wrap up what it's doing so it doesn't get messy. There are a number of built in CFML functions like `sleep()` that will automatically check and see if the thread they are executing in has been interrupted. A number of build in CommandBox functions like the print helper also check to see if the thread has been interrupted.

These method throw an `InterruptedException` which aborts the execution of your task and rolls back to the interactive command prompt. But what if your task is doing a lot of work and it doesn't realize it's been asked to stop? If your task does a very large amount of computations in a loop of some kind, you can periodically check if the user has tried to interrupt you by calling this built in method that is available to all custom commands and Task Runners.

```text
checkInterrupted();
```

If the current thread hasn't been interrupted, that call will simply return immediately and you can continue with your work. If your thread has been interrupted, that call with throw an exception. No need to catch it-- the exception will automatically stop execution of your task and the CommandBox shell will catch the exception itself, output the text "CANCELLED" and return to the prompt.

If you do call this method from inside of a try/catch, you'll want to rethrow any interrupted exceptions.  Also, if your work has any cleanup that must always be performed like closing a socket connection, make sure you use a `finally {}` block for those items.

