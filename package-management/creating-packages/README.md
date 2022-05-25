# Creating Packages

Packages are quite simply a folder that contains some code and a `box.json` file. A package can be a simple CFC, a self-contained library, or even an entire application. ColdBox and ContentBox modules also make great "smart" packages.

Remember, packages aren't just the things you install **into** your application, but your application **is** a package too! That's why when you install something in your app, we'll create a /box.json if it doesn't exist to start tracking your dependencies.

Your `box.json` file describes your package, dependencies, and how to install it. To turn a boring folder into a sweet package just run the `init` command in the root of the folder.

```
init name="My Package" version="1.0.0"
```

That's it. Your folder now has extra meta data in the `box.json` file that describes it in a way that is meaningful to [ForgeBox](http://forgebox.io) and CommandBox.

## Distribution

When making a package available on ForgeBox, each version of that package has its own location. Most download locations point to a zip file, that when extracted, contains a folder with a box.json in it. The box.json designates the root of the package. However, the `location` property of your box.json can be any valid endpoint ID. An example would be:

```javascript
{
  "name":"my project",
  "slug":"my-project",
  "version":"1.0.0",
  "location":"githubUser/repoName#v1.0.0"
}
```

In that case, the `location` for version `1.0.0` of this package is the `v1.0.0` tag in that GitHub repository.

## Storing Package Binaries on ForgeBox

ForgeBox can store the binaries for your packages in the ForgeBox Cloud. This provides you with an easy way to store multiple versions of your package distributed across the globe.

To utilize ForgeBox Storage, simply set `forgeboxStorage` as the value of your package's `location`.

```bash
package set location=forgeboxStorage
```

When you publish a package, CommandBox will automatically zip up your package and send it to ForgeBox.

## Forgebox `publish` Command

When you run the `publish` command from the root of a package, the package will be created on ForgeBox.  If the package already exists in ForgeBox, the new version will be added.  If the version already exists, the package metadata will be updated. &#x20;

Most of the data about a package exists in the `box.json` such as name, slug, version, etc.  There are also some files read from disk by convention.  The `publish` command looks by convention for the following files (case insensitive) when publishing

* `readme` - Maps to Package Description
* `instructions` - Maps to Installation Instructions
* `changelog` - Maps to Change Log

Every file is checked for `.md`, `.txt`, and no extension in that order. &#x20;



## Publishing to ForgeBox from start to finish

Below is an example of the commands that would take you from scratch to a published package:

```bash
# Create user (first time only)
forgebox register username password your@email.com firstName lastName
forgebox login username password

# Create package
mkdir mypackage --cd
package init slug=my-package type=modules
bump --major

# Publish it
publish

# Viewable and installable by the world!
forgebox show my-package
install my-package
```

## Private Packages

ForgeBox supports private packages. Private packages are only visible to the user who created it.

To create a private package, pass the `private` flag to the `package init` command.

```bash
CommandBox:my-package> package init slug=my-package --private
- Set name = My Package
- Set slug = my-package@username
- Set version = 0.0.0
- Set private = true
- Set shortDescription = A sweet package
- Set ignore = ["**/.*","test","tests"]
Package Initialized & Created /Users/username/code/sandbox/my-package/box.json
```
