# Checksums
Another very common requirement for builds is generating checksums on your files.  We've got you covered here now as well.  Run `checksum help` for even more options.

## From the CLI

```
checksum file.txt
checksum path=build.zip algorithm=SHA-256
```

## From CFML
```js
command( 'checksum' )
    .params( 'file.txt' )
    .run();

command( 'checksum' )
    .params( path = 'build.zip', algorithm = 'SHA-256' )
    .run();
```

## Checksum all files in a directory

You can provide a file globbing pattern to receive a checksum for all files in a directory that match that pattern.

```
checksum *.cfc
```

## Output format

The `checksum` command also supports some other popular formats for outputting checksums.  The default format is `checksum`.
```
checksum path=**.cfc format=checksum
checksum path=**.cfc format=sfv
checksum path=**.cfc format=md5sum
```

## Verify a file against an existing checksum

You can check a file against an existing checksum to make sure the file hasn't changed.

```

```