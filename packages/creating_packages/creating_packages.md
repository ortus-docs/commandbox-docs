# Creating Packages

Packages are quite simply a folder that contains some code and a `box.json` file. A package can be a simple CFC, a self-contained library, or even an entire application. ColdBox and ContentBox modules also make great "smart" packages.

Remember, packages aren't just the things you install **into** your application, but your application **is** a package too!  That's why when you install something in your app, we'll create a /box.json if it doesn't exist to start tracking your dependencies. 

Your `box.json` file describes your package, dependencies, and how to install it. To turn a boring folder into a sweet package just run the `init` command in the root of the folder.

```
init name="My Package" version="1.0.0"
```

That's it.  You can now commit this package to [ForgeBox](http://forgebox.io) and can be available world-wide.

## Distribution

When making a package available on ForgeBox, the download URL should point to a zip file, that when extracted, contains a folder with a box.json in it.  The box.json designates the root of the package.  

If your project is stored in GitHub, an easy approach is simply to treat the root of the repository as the root of the package.  That is where your box.json will live.  This also means you can use GitHub's automatic zip download URL as your ForgeBox URL since it returns a zip file containing your repo contents in a folder.

Ex:
`https://github.com/bdw429s/Weather-Lookup-By-IP/archive/master.zip`

If you choose  to structure your repo differently, no problem.  Just use a build process that generates a zip file in that format and make that zip publicly available for ForgeBox's download URL.

## Publishing to ForgeBox

### Creating a User from CommandBox

To create a new ForgeBox user right from the CLI run the "forgebox register" command.

### Authenticating from CommandBox

Once you have a confirmed user, you can run "forgebox login" to authenticate your account.  This will store your API key in a CommandBox config setting.  You can see it with:

```bash
CommandBox> config show endpoints.forgebox
```

### Publish Packages

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


### Unpublish
Sometimes you want to remove a specific package version or an entire package from ForgeBox. For this, use the `unpublish` command. 
> Warning 

### Publishing to ForgeBox from start to finish

Below is an example of the commands that would take you from scratch to a published package:

```bash
# Create user (first time only)
CommandBox> forgebox register username password your@email.com firstName lastName
CommandBox> forgebox login username password

# Create package/git repo
CommandBox> mkdir mypackage --cd
CommandBox> !git init
CommandBox> package init slug=my-package type=modules location=gitUser/my-package
CommandBox> bump --minor message="Initial Commit"

# Publish it
CommandBox> !git remote add origin <git url>
CommandBox> !git push
CommandBox> publish

# Viewable and installable by the world!
CommandBox> forgebox show my-package
CommandBox> install my-package
```