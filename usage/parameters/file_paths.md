# File Paths
Many Commands accept a path to a folder or file on your hard drive. You can specify a fully qualified path that starts at your drive root, or a relative path that starts in your current working directory. To find your current working directory, use the `pwd` command (Print Working Directory). To change your current working directory, use the `cd` command.

Here is a fully qualified path in Windows and *nix-based:

### Windows:
```bash
mkdir C:/sites/test
mkdir /sites/test
mkdir \\server/share/test
```

Note that if you start a path with a single leading slash in Windows, it will be an absolute path using the drive letter of the current working directory.  

### *nix:
```bash
mkdir /opt/var/sites/test
```

For a relative path, do not begin with a slash.

```
mkdir test
```

File system paths will be canonicalized automatically which means the following is also valid:

```bash
mkdir ../../sites/test
```
