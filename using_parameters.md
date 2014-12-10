# Using Parameters

Regardless of whether your command is called with named parameters, positional parameters, or boolean flags, you'll access them the same way: via the standard CFML arguments scope.  The user will be prompted for required parameters if they don't provide them, and defaults you configure will also work just like you expect.

If the parameters were escaped when typed into the command line, you will receive the final unescaped version in your command.

## File System Paths

If your command accepts a file or folder path from the user, you'll want to resolve that path before you use it.  This ensures you have a fully qualified path to work with even though the user might have typed a folder relative to their current working directory or used tricks such as `../../` which need to be canonicalized.  

To do this, use the `fileSystemUtil` that is available to all commands via the BaseCommand class.  The method resolvePath() will make the file system path canonical and absolute.

```javascript
component extends="commandbox.system.BaseCommand" {

	function run( String directory )  {
		// This will make each directory canonical and absolute
		arguments.directory = fileSystemUtil.resolvePath( arguments.directory );
		return arguments.directory;
	}
}
```

So if yor run that command and pass a full file path such as `C:\sandbox\testSite` you would get that exact same path back as the output.  

However, if you changed the interactive shell to the `C:\sandbox` directory and then ran the command with `testsite` as the input, the relative path would now still resolve to `C:\sandbox\testSite`.

Now if from the same direcotory, you passed `testsite/foo/bar/../../` you would still get `C:\sandbox\testSite` as the path.