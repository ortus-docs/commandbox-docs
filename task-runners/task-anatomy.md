# Task Anatomy

In the previous section, we built our first task and executed it. Let's take a look at how the simple conventions work for task runners.

## Default conventions

The `task run` command will look for a default task name of `task.cfc` in the current working directory. The default target \(or method\) is called `run`. Therefore, when you run

```text
task run
```

you will execute the `run()` method of `./task.cfc` relative to your current working directory.

## Take you to task

A task is just a CFC file. The task CFCs will extend the `commandbox.system.BaseTask` class, but it isn't necessary for your to extend it manually. CommandBox will add in virtual inheritance for you. The task CFCs can live wherever you want in your project \(or outside of it!\) and can have any name you wish. Since tasks are not scanned and registered by CommandBox, but rather created on-the-fly by convention, you don't have to worry about more than one task in different projects with the same name. That means, each of your projects can have a `workbench/build.cfc` task runner and they won't collide. When you run the task, the correct CFC will be located based on your current working directory. Therefore, tasks are not "global" like commands, but rather in the context of a given project or folder.

**myTaskName.cfc**

```javascript
component {
  function run() {
    // Do awesome task stuff here
  }
}
```

To call the above task, we'd reference the path to where the CFC lives. It isn't necessary to include the `.cfc` part of the name, though we'll forgive you if you do!

```text
task run path/to/myTaskName
```

## Hit the target

You can have as many targets in your task as you wish, by simply declaring public methods. A task can have no parameters or as many as you like. Default argument values will be used as well. It's just a method execution, but from the command line!

**build.cfc**

```javascript
component {

  function createZips() {
    print.line( 'Zips created!' );
  }

  function compileAssets(){
    print.line( 'Assets Compiled!' );
  }

  function minimizeJS(){
    print.line( 'JS Minimized!' );
  }

}
```

To run each of the targets above, you'd type this:

```text
task run build createZips
task run build compileAssets
task run build minimzeJS
```

## WireBox DI

All CFCs including tasks are created and wired via WireBox, so dependency injection and AOP are available to them. This can be handy for tasks to wrap services provided by models, or to access utilities and services inside CommandBox.

This task would inject CommandBox's ArtifactService to list out all the packages being stored.

```javascript
component {

    property name='artifactService' inject='artifactService';

    function run(){
        var results = artifactService.listArtifacts();
        for( var package in results ) {
            print.boldCyanLine( package );
        }
    }

}
```

Tasks also have a `variables.wirebox` variable as well as their own `getInstance()` method which proxies to WireBox to get objects.

```javascript
var results = getInstance( 'artifactService' ).listArtifacts();
```

## Threading

The CommandBox CLI is implemented as a single long request in the underlying Lucee server. Due to this it is required to make a unique thread name every call to the task or else an error can occur. The following is an example you can use for running threads.

```javascript
var threadName = createGUID();
cfthread( action="run" name=threadName) {
  //Thread body
}
cfthread( action="terminate" name=threadName);
```

## Sending Email

To send email, use the `cfscript` variant of `cfmail` making sure you set `asyc=false` (see below). Not setting this flag to `false` may result in undelivered email because mail may still exist in Lucee spooler (Lucee tasks) when your task runner exits.

```javascript
mail 
    subject=subject 
    from=from 
    to=to
    cc=cc
    bcc=bcc
    replyTo=replyTo
    type=type
    server=server
    port=port
    username=username
    password=password
    useTls=tls
    usessl=ssl
    async=false {
writeOutput(emailBody);
};
```

### Connecting to SMTP Services Using SSL or TLS

If you cannot connect to a SMTP server that requires `SSL` or `TLS`, like [Amazon SES](https://aws.amazon.com/ses/), one workaround is to install a local SMTP server and configure it as a relay to your SMTP server. This has been done successfully on Windows servers using [hMailServer](https://www.hmailserver.com/) (free, opensource), which is fairly easy to install and configure as an SMTP relay.

## Modifying Application Settings

Task Runners do not execute a `application.cfc` or `application.cfm`, but you can use the [cfapplication](https://docs.lucee.org/reference/tags/application.html) tag \(or script variant: `application`\) to modify the properties and behaviors of the Task Runner application. Any setting that can be modified using [cfapplication](https://docs.lucee.org/reference/tags/application.html) can be modified in Task Runners as follows:

```javascript
// to create a datasource, first get the application settings
appSettings = getApplicationSettings();

// initialize with the current value of application datasources
dsources = appSettings.datasources ?: {};

// add a new datasource to it
dsources[ 'myNewDS' ] = { ... };

// call cfapplication (cfscript variant) to update the datasources and set AWS S3 credentials
application action="update" datasources=dsources s3={} /* and any other settings */ ;
```

You can also define mappings as follows:

```javascript
fileSystemUtil.createMapping( name, physicalpath );
```

**NOTE:** The settings that are changed using `cfapplication` will last for the duration of the CLI shell and will affect any and all code run from the CLI including the CommandBox core code.

