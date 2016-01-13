#JVM Args

The following JVM Args are supported when starting the embedded server.

## heapSize

You can set the max heap size the server is allowed to have `-Xmx` by passing the `heapSize` parameter to the `start` command.  This parameter is always specified as megabytes. 

```bash
start heapSize=1024
```

In `server.json`

```bash
server set heapSize=1024
server show heapSize
```

## Ad Hoc JVM Args

You can specify ad-hoc JVM args for the server with the `JVMArgs` parameter.

```bash
start JVMArgs="-XX:MaxGCPauseMillis\=200"
```
In `server.json`

```bash
server set JVMArgs="-XX:MaxGCPauseMillis\=200"
server show JVMArgs
```

## Ad Hoc Runwar Options

You can specify ad-hoc options for the underlying Runwar library using the `RunwarArgs` parameter.

```bash
start RunwarArgs="--sendfile-enable false"
```

In `server.json`


```bash
server set runwarArgs="--sendfile-enable false"
server show runwarArgs
```