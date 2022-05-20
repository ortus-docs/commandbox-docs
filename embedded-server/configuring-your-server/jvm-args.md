# JVM Args

The following JVM Args are supported when starting the embedded server.

## heapSize

You can set the max heap size the server is allowed to have (`-Xmx`) by passing the `heapSize` parameter to the `start` command. This parameter defaults to megabytes but you can specify any valid suffix.

```bash
start heapSize=1024
```

In `server.json`

```bash
server set JVM.heapSize=1024
server set JVM.heapSize=2G
server show JVM.heapSize
```

## minHeapSize

You can set the starting heap size for the server (`-Xms`) by passing the `minHeapSize` parameter to the `start` command. This parameter defaults to megabytes but you can specify any valid suffix.

```bash
start minHeapSize=1024
```

In `server.json`

```bash
server set JVM.minHeapSize=1024
server set JVM.minHeapSize=2G
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

JVM args can also be set an array of strings which prevents you from needing to escape or quote anything. &#x20;

```
{
  "jvm" : {
    "args" : [
       "-XX:+UseG1GC",
       "-XX:-CreateMinidumpOnCrash",
       "--add-opens=java.base/java.net=ALL-UNNAMED"
    ]
  }
}
```

Or via the CLI like so:

```
server set jvm.args=["-XX:+UseG1GC"]
server set jvm.args=["-XX:-CreateMinidumpOnCrash"] --append
server set jvm.args=["--add-opens=java.base/java.net=ALL-UNNAMED"] --append
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

Runwar args can also be set an array of strings which prevents you from needing to escape or quote anything. &#x20;

```
{
  "runwar" : {
    "args" : [
      "--runwar-option",
      "value",
      "--runwar-option2",
      "value2"
    ]
  }
}
```

Or via the CLI like so:

```
server set runwar.args=["--runwar-option","value"]
server set runwar.args=["--runwar-option2","value2"] --append
```
