# JVM Args

The following JVM Args are supported when starting the embedded server.

## heapSize

You can set the max heap size the server is allowed to have \(`-Xmx`\) by passing the `heapSize` parameter to the `start` command. This parameter is always specified as megabytes.

```bash
start heapSize=1024
```

In `server.json`

```bash
server set JVM.heapSize=1024
server show JVM.heapSize
```

## minHeapSize

You can set the starting heap size for the server \(`-Xms`\) by passing the `minHeapSize` parameter to the `start` command. This parameter is always specified as megabytes.

```bash
start minHeapSize=1024
```

In `server.json`

```bash
server set JVM.minHeapSize=1024
server show JVM.minHeapSize
```

## Ad Hoc JVM Args

You can specify ad-hoc JVM args for the server with the `JVMArgs` parameter.

```bash
start JVMArgs="-XX:MaxGCPauseMillis\=200"
```

In `server.json`

```bash
server set JVM.args="-XX:MaxGCPauseMillis\=200"
server show JVM.args
```

## Ad Hoc Runwar Options

You can specify ad-hoc options for the underlying Runwar library using the `RunwarArgs` parameter.

```bash
start RunwarArgs="--sendfile-enable false"
```

In `server.json`

```bash
server set runwar.args="--sendfile-enable false"
server show runwar.args
```

