# Using Parameters

Regardless of whether your command is called with named parameters, positional parameters or boolean flags, you'll access them the same way: via the standard CFML arguments scope. The user will be prompted for required parameters if they haven't provided them, and the defaults you configured will also work just like you expect.

If the parameters were escaped when typed into the command line, you will receive the final unescaped version in your command.

## Dynamic Parameters

Users can pass named or positional parameters that aren't declared, and they will come through the `arguments` scope.  Named parameters will be accessable as `arguments.name`, and positional parameters as `arguments[ 1 ]`, `arguments.[ 2 ]`, etc.  

This can allow for powerful commands like `package set` that allows users to set any box.json property they want.

```bash
package set foo=bar
```

## File System Paths As Parameters

If your command accepts a file or folder path from the user, you'll want to resolve that path before you use it. To do this, use the `fileSystemUtil` object that is available to all commands via the BaseCommand class. The method `resolvePath()` will make the file system path canonical and absolute. This ensures you have a fully qualified path to work with even if a user might passed a folder relative to their current working directory passed something like `../../`.

```javascript
component extends="commandbox.system.BaseCommand" {

	function run( String directory )  {
		// This will make each directory canonical and absolute
		arguments.directory = fileSystemUtil.resolvePath( arguments.directory );
		return arguments.directory;
	}
}
```

If you run that command and pass a full file path such as `C:\sandbox\testSite`, you would get that exact same path back as the output.

However, if you changed the interactive shell to the `C:\sandbox` directory and then ran the command with `testsite` as the input, the relative path would now still resolve to `C:\sandbox\testSite`.

If, from the same directory, you passed `testsite/foo/bar/../../`, you would still get `C:\sandbox\testSite` as the path.

> **Warning** Path resolution follows the users' OS rules, not CFML rules.  Ex, any path starting with a forward slash will be considered absolute on Unix, but relative to the CWD on Windows.