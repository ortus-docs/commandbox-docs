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