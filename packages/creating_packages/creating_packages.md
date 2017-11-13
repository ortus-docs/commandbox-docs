# Creating Packages

Packages are quite simply a folder that contains some code and a `box.json`
file. A package can be a simple CFC, a self-contained library, or even an entire
application. ColdBox and ContentBox modules also make great "smart" packages.

Remember, packages aren't just the things you install **into** your application,
but your application **is** a package too! That's why when you install something
in your app, we'll create a /box.json if it doesn't exist to start tracking your
dependencies.

Your `box.json` file describes your package, dependencies, and how to install
it. To turn a boring folder into a sweet package just run the `init` command in
the root of the folder.

```
init name="My Package" version="1.0.0"
```

That's it. Your folder now has extra meta data in the `box.json` file that
describes it a way that is meaningful to [ForgeBox](http://forgebox.io) and
CommandBox.

## Distribution

When making a package available on ForgeBox, each version of that package has
its own location. Most download locations point to a zip file, that when
extracted, contains a folder with a box.json in it. The box.json designates the
root of the package. However, the `location` property of your box.json can be
any valid endpoint ID. An example would be:

```js
{
  "name":"my project",
  "slug":"my-project",
  "version":"1.0.0",
  "location":"githubUser/repoName#v1.0.0"
}
```

In that case, the `location` for version `1.0.0` of this package is the `v1.0.0`
tag in that GitHub repository.

If your project is stored in GitHub, an easy approach is simply to treat the
root of the repository as the root of the package. That is where your box.json
will live. This also means you can use GitHub's automatic zip download URL as
your ForgeBox URL since it returns a zip file containing your repo contents in a
folder.

Ex: `https://github.com/bdw429s/Weather-Lookup-By-IP/archive/master.zip`

If you choose to structure your repo differently, no problem. Just use a build
process that generates a zip file in that format and make that zip publicly
available for ForgeBox's download URL.

## Private Packages

ForgeBox supports private packages. Private packages are only visible to the
user who created it.

To create a private package, pass the `private` flag to the `package init`
command.

> Note: Creating private packages requires you to be logged in to ForgeBox in

    CommandBox.

```bash
CommandBox:my-package> package init slug=my-package --private
- Set name = My Package
- Set slug = @username/my-package
- Set version = 0.0.0
- Set private = true
- Set shortDescription = A sweet package
- Set ignore = ["**/.*","test","tests"]
Package Initialized & Created /Users/username/code/sandbox/my-package/box.json
```

## Publishing to ForgeBox from start to finish

Below is an example of the commands that would take you from scratch to a
published package:

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
