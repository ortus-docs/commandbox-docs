# Publishing to ForgeBox


## Publish Packages

The "forgebox publish" command (aliased as "publish") will add a new package to ForgeBox or update an existing one (that belongs to you). If the local version number is different, a new version will be created in ForgeBox.

ForgeBox does not store your actual package files like npm.  We just point to your download location.  We've been optimizing this as much as possible to work seamlessly with a Git repo. When you publish a package, this data is sent to the server:
 
* Your box.json.  Specifically
	* name
	* slug
	* version
	* type
	* location (this is where to download your package from and can be an HTTP URL or ANY VALID package ID such as `repoUser/repoName#v1.2.3`)
	* etc
* The contents of your readme[.txt|.md] file, if one of those exists
* The contents of your changelog[.txt|.md] file, if one of those exists
* The contents of your instructions[.txt|.md] file, if one of those exists
* Your API key config setting to authenticate

That's it.  Once you run this command, you can run `forgebox show my-package` to confirm it's there. Any updates to your readme, title, etc. will overwrite the old data. If you change the slug, a new package will be created. If you change the version, a new version will be added to the existing package.


## Unpublish
Sometimes you want to remove a specific package version or an entire package from ForgeBox. For this, use the `unpublish` command. 
>**Warning** Only remove a package if you're completely sure no one is using it.  Otherwise you may be breaking someone else's app!  Also, it's best to never re-use a version number.

```bash
# Remove all versions of a package
CommandBox> unpublish
# Remove a specific version of a package
CommandBox> unpublish 1.2.3
# Skip the user confirmation prompt
CommandBox> unpublish 1.2.3 --force```
