# Undertow Options

CommandBox uses JBoss Undertow to power it's lightweight servlet containers.  Undertow also powers JBoss Wildfly and has a lot of configurable options, not all of which have first-class CommandBox settings.  These low-level settings come in two different categories:

* **Undertow Options** - Settings that apply to the servlet and web server aspects of Undertow
* **XNIO Options** - Part of the underlying XNIO library which powers all low-level I/O in undertow

## Undertow Options

Undertow has its own set of options which can be found here:

{% embed url="http://undertow.io/javadoc/2.0.x/io/undertow/UndertowOptions.html" %}

To set an XNIO option that CommandBox doesn't already expose with a first-class setting, you can set them into your `server.json` like so:

```bash
server set runwar.undertowOptions.ALLOW_UNESCAPED_CHARACTERS_IN_URL=true
```

You can also set global XNIO objects that will apply to all servers.  Global options will be appended to server-level options.

```bash
config set server.defaults.runwar.undertowOptions.WORKER_NAME=myWorker
```

## XNIO Options

XNIO (which is a refined version of NIO (Non-blocking I/O) has its own set of options that apply to the low level network transport functions it provides.  You can find the full set of XNIO options here:

{% embed url="https://javadoc.io/doc/org.jboss.xnio/xnio-api/latest/org/xnio/Options.html" %}

To set an XNIO option that CommandBox doesn't already expose with a first-class setting, you can set them into your `server.json` like so:

```bash
server set runwar.XNIOOptions.WORKER_NAME=myWorker
server set runwar.XNIOOptions.SSL_ENABLED_PROTOCOLS=TLSv1.3,TLSv1.2
```

You can also set global XNIO objects that will apply to all servers.  Global options will be appended to server-level options.

```bash
config set server.defaults.runwar.XNIOOptions.WORKER_NAME=myWorker
config set server.defaults.runwar.XNIOOptions.SSL_ENABLED_PROTOCOLS=TLSv1.3,TLSv1.2
```
