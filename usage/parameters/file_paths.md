# File Paths
Many Commands accept a path to a folder or file on your hard drive. You can specify a fully qualified path that starts at your drive root, or a relative path that starts in your current working directory. To find your current working directory, use the `pwd` command (Print Working Directory). To change your current working directory, use the `cd` command.

## Absolute

Here are examples of fully qualified paths in Windows and *nix-based:

### Windows:
```bash
mkdir C:/sites/test
mkdir /sites/test
mkdir \\server/share/test
```

>**Info** Note that if you start a path with a single leading slash in Windows, it will be an absolute path using the drive letter of the current working directory.  

### *nix:
```bash
mkdir /opt/var/sites/test
```

## Relative

For a relative path, do not begin with a slash.

```
mkdir test
```

File system paths will be canonicalized automatically which means the following is also valid:

```bash
mkdir ../../sites/test
```

## UNC Server paths

If you are on Windows, CommandBox supports UNC server shares bu accessing the name of the server or the IP address.  Note, you can use forward or backslashes in Windows, but a UNC path MUST start with two backslashes.  

```bash
\\server/share/sites/test
\\10.10.1.205/webroot/logs
```

Backslashes need to be escaped   from the command line and in JSON, so most usage of UNC server paths will require you to type four slashes like `\\\\` which will properly escape to `\\`

```bash
cd \\\\server/share/sites/test
mkdir \\\\10.10.1.205/webroot/logs
```

You cannot directly list the contents of a server like `\\webdev01` as that is not a true directory.  You always need to access a specific share like `\\webdev01/webroot`.

### Permissions

By default, CommandBox will access UNC paths using the same permissions of the user that the `box` process was started with.  There's no way to specify a user, so if you need to use a custom user, you'll need to run native `NET USE` command from the CLI first to change how it is authenticating.  Unfortunately, this is a limitation of how Java accesses UNC paths so CommandBox has little control over it.