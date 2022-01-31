# Custom Java Version

By Default, your servers start using the same version of Java that the CommandBox CLI is using. For people needing to run Adobe ColdFusion 9, or who just want to do some testing on different JREs, you can point each of your servers at a custom JRE and CommandBox will use it when starting the server.

if you already have a JRE downloaded somewhere on your hard drive, you can manually point the server at it, or you can simply tell CommandBox which version of java you'd like, at it will automatically download that version of OpenJDK for your server to use \(if it's not already downloaded\)

## Manual Java Config

Point CommandBox to an existing java install like so:

```bash
server set jvm.javaHome="C:\Program Files\Java\jdk1.8.0_25"
```

To set the default version of Java for all the servers you start on your machine, use the global config setting defaults.

```bash
config set server.defaults.jvm.javaHome="C:\Program Files\Java\jdk1.8.0_25"
```

## Automatic Java Download

To let CommandBox take over and acquire Java for you, pass an installation endpoint ID to the `start` command

```bash
server start javaVersion=openjdk8
```

or set it in your `server.json`

```bash
server set jvm.javaVersion=openjdk8
```

or set a default for all servers

```bash
config set server.defaults.jvm.javaVersion=openjdk8
```

To review what possible IDs you can use to dial in your exact Java version, read the [docs on our Java endpoint](../../package-management/code-endpoints/java.md#installation-id). You don't need to manually install Java  CommandBox will do that for you. You just need to provide a valid ID so CommandBox knows what you want.

In some cases you might not want to interact with the command line manually to setup an environment, in particular in production setups. If you prefer to setup your server via a server.json file, the same syntax for valid IDs applies to setting your JVM ID:

```json
{
    "JVM":{
        "javaVersion":"openjdk11_jre_x64_linux_hotspot_jdk-11.0.13+8"
    }
}
```

## Java Namespace

To make it easier for you to manage the Java installations CommandBox makes for you, we have a namespace of commands you can use. The Java versions CommandBox installs automatically for your servers to use are stored in a folder under your CommandBox home. CommandBox manages this folder for you. You can change where the system Java installation go like so:

```bash
config set server.javaInstallDirectory=C:/path/to/JREs/
```

### java search

Search the AdoptOpenJDk API for available versions of Java for you to use.

```bash
java search version=openjdk8
```

You can filter the version, jvm, os, CPU arch, type, and release. Most of those parameters default to match your local system. For instance, running this command on Windows will only return Windows versions. To open up the search, pass nothing to that filter.

```bash
java search version=openjdk8 os=
```

### java list

List the installed Java installations for you to start servers with. If you have set a global default Java version it will be marked in the list.

```bash
java list
```

### java setDefault

You may change the global default Java version for your servers with this command.

```bash
server java setDefault openjdk11
```

The ID follows the format from the [Java endpoint](../../package-management/code-endpoints/java.md#installation-id). If the version you set as the default isn't installed yet, CommandBox will install it for you the next time a server starts or you can use the `--install` flag.

### java install

You can pre-install a Java version so it's ready to go the next time you start a server with this command. This differs from the normal `package install` command in that it doesn't install to the current working directory, but into the core server JRE folder that CommandBox manages for you. Use the `--setDefault` flag to also set the newly installed Java version as the global default for all servers.

```bash
server java install openjdk11 --setDefault
```

### java uninstall

You can remove a java installation so it doesn't take up space on your hard drive. Use the FULL ID that shows in the `java list` command to uninstall.

```bash
server java uninstall openjdk9_jre_x64_windows_hotspot_jdk-9.0.4+11
```

Note, the download will still be in your artifacts cache. Also, if you start a server up again that asks for a Java installation you've uninstalled, CommandBox will simply re-install it again.

