#JVM Args

The following JVM Args are supported when starting the embedded server.

## heapSize

You can set the max heap size the server is allowed to have `-Xmx` by passing the `heapSize` parameter to the `start` command.  This parameter is always specified as megabytes. 

```bash
start heapSize=1024
```

## Ad Hoc JVM Args

You can specify ad-hoc JVM args for the server with the `JVMArgs` parameter.

```bash
start JVMArgs="-myRandomArg=123"
```

## Ad Hoc Runwar Options

You can specify ad-hoc options for the underlying Runwar library using the `RunwaArgs` parameter.

```bash
start JVMArgs="--myRandomArg=123"
```