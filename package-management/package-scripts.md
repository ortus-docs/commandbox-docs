# Package Scripts

You can define a struct in your `box.json` called `scripts` that correspond to the interception points in CommandBox. Any script defined there will be ran on during that interception point. This allows you to prescribe arbitrary commands on a package-by-package basis. You may wish to set your package location any time you `bump` a package version or perform a `!git push` on `publish`. Below is an example of some `scripts` in a `box.json`.

```javascript
{
  "name" : "My Package",
  "slug" : "my-package",
  "version" : "1.0.0",
  "scripts" : {
   "postVersion" : "package set location='gitUser/gitRepo#v`package version`'",
   "postPublish" : "!git push --follow-tags"
  }
}
```

Package scripts can be named after any valid interception point and can contain any valid command that you can run from the interactive shell.

## Ad-hoc package scripts

You can also create ad-hoc scripts with arbitrary names that contain a collection of often-run commands.

```javascript
{
  "name" : "My Package",
  "slug" : "my-package",
  "version" : "1.0.0",
  "scripts" : {
   "build" : "!grunt build && testbox run && run-script generateAPIDocs && bump --patch && publish",
   "generateAPIDocs" : "docbox generate"
  }
}
```

Call these scripts at any time with the `run-script` command

```bash
run-script generateAPIDocs
run-script build
```

## pre/post package scripts

Before any package script is run, CommandBox will look for another package script with the same name, but prefixed with `pre`. After any package script is run, CommandBox will look for another package script with the same name, but prefixed with `Post`. So if you have a package that contains 3 package scripts: `foo`, `preFoo`, and `postFoo`, they will run in this order.

1. preFoo
2. foo
3. postFoo

This works for built-in package script names as well as as doc package scripts. It also works on any level. In the example above, if you created a 4th package script called `prePreFoo`, it would run prior to `preFoo`.

## Access Intercept Data

Any package script fired by an internal interception announcement in CommandBox will have access to any intercept data via environment variables in the shell.  All intercept data will be prefixed with `interceptData.` and will use "dot notation" for nested structs.  You can see if the docs on what intercept data is available to each interception point.  

For example, a `preCommand` interception announcement receives a struct called `commandInfo` with a key called `commandString` which means your package script can access that via the following environment variable:

```text
${interceptData.commandinfo.commandString}
```

You can see this in action with this simple package script:

```text
package set scripts.preCommand="echo 'You are running [\${interceptData.commandinfo.commandString}]'"
```

Or if you wanted to simply debug what is available to you, use the `env show` command in your package script to dump out all environment variables to the console.  

Here's another example that writes a file to the server home directory when a server starts, using an environment variable to dynamically obtain the proper path.

```text
package set scripts.onServerStart="touch \${interceptData.serverInfo.serverHomeDirectory}/hi.txt"
```



