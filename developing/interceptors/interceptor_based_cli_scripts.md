# Interceptor-based CLI scripts

You can define a struct in your `box.json` called `scripts` that correspond to the interception points in CommandBox.  Any script defined there will be ran on during that interception point.  This allows you to prescribe arbitrary commands on a package-by-package basis.  You may wish to set your package location any time you `bump` a package version, perform a `!git push` on `publish`, start a server, or any other valid CommandBox command.  Below is an example of some `scripts` in a `box.json`.

```json
{
  "name" : "My Package",
  "slug" : "my-package",
  "version" : "1.0.0",
  "scripts" : {
   "postVersion" : "package set location='gitUser/gitRepo#`package version`'"
   "postPublish" : "!git push"
  }
}
```