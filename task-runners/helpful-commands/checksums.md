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