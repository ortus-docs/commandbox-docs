# Interceptor-based CLI scripts

You can define a struct in your `box.json` called `scripts` that correspond to the interception points in CommandBox.  Any script defined there will be ran on during that interception point.  This allows you to prescribe arbitrary commands on a package-by-package basis.  You may wish to set your package location any time you `bump` a package version or perform a `!git push` on `publish`.  Below is an example of some `scripts` in a `box.json`.

```json
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

```json
{
  "name" : "My Package",
  "slug" : "my-package",
  "version" : "1.0.0",
  "scripts" : {
   "build" : "!grunt build && testsbox run && run-script generateAPIDocs && bump --patch && publish",
   "generateAPIDocs" : "docbox generate"
  }
}
```

Call these scripts at any time with the `run-script` command

```bash
run-script generateAPIDocs
run-script build
```
