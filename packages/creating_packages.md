# Creating Packages

Packages are quite simply a folder that contains some code and a `box.json` file. A package can be a simple CFC, a self-contained library, or even an entire application. ColdBox and ContentBox modules also make great "smart" packages.

Your `box.json` file describes your package, dependencies, and how to install it. To turn a boring folder into a sweet package just run the `init` command in the root of the folder.

```
init name="My Package" version="1.0.0"
```

That's it.  You can now commit this package to ForgeBox and can be available world-wide.