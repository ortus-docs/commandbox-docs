#JVM Args

The following JVM Arg is supported when starting the embedded server.

## heapSize

You can set the max heap size the server is allowed to have `-Xmx` by passing the `heapSize` parameter to the `start` command.  This parameter is always specified as megabytes. 

```bash
start heapSize=1024
```