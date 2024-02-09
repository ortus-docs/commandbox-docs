# Java

The Java endpoint is capable of downloading and installing any version of Java in the AdoptOpenJDK API. Use the `java search` command to search the API to find what Java versions are available. The endpoint will use the local artifacts cache.

```bash
java search version=openjdk8 release=
```

**NOTE:** If you simply want to start a server with a given version of OpenJDK, you don't need to use this endpoint directly, You can [_ask CommandBox to use a specific java version_](../../embedded-server/configuring-your-server/custom-java-version.md#automatic-java-download) and it will handle the installation for you as well as re-using identical Java installs across servers. For example:

```bash
java install OpenJDK8_jdk8u181b13
```

## Installation ID

The installation ID consists of a series of identifiers separated by underscores that describe the Java version, type, JVM, etc that you want installed.

```bash
install java:<version>_<type>_<arch>_<os>_<jvm-implementation>_<release>
```

An example utilizing every option would be

```bash
install java:OpenJDK8_jre_x64_windows_hotspot_jdk8u181b13
```

However, everything is optional except the major version. These are also valid installation IDs

```bash
install java:openjdk8
install java:openjdk8_jre
install java:OpenJDK8_jdk8u181b13
install java:OpenJDK8_jdk_jdk8u181b13
```

## ID Definitions

Here are the possible options for each section of the ID.

* **version** - openjdk8, openjdk9, openjdk10, etc...
* **type** - jdk, jre
* **arch** - x64, x32, ppc64, s390x, ppc64le, aarch64, arm etc...
* **os** - windows, linux, mac, solaris, alpine\_linux (openjdk11+ only)
* **jvm-implementation** - hotspot
* **release** - latest, jdk8u172, jdk8u172-b00, etc...

## Defaults

For items not specified in the endpoint ID, the endpoint will default as follows:

* **type** - jre
* **arch** - Whatever the current OS arch is
* **os** - Whatever the current OS is
* **JVM**-implementation - hotspot
* **release** - latest

## Installation Folder

When java is installed as a package endpoint, the name of the folder that it is installed into will mirror the ID you use. This means two different installation IDs that resolve to the exact same Java version may end up in different folders. We do this for a couple reasons. One of which is simplicity, the other is that if you have configuration expecting a specific folder name, Mac users and Windows users may get a different download, but if the id is simple `java:openjdk8_jdk`, they would both install into a folder called `openjdk8_jdk`.

This endpoint will NOT edit your registry, create any symlinks, or add environment variables. It will simply drop the Java installation into a folder so you can point servers at it. You are free to use one of these Java installations in such a manner, but we don't do this by default so you can install as many java versions as you like and they don't fight with each other.
