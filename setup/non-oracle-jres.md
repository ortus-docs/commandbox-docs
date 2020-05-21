# Non-Oracle JREs

In the past, 99% of people used the Oracle \(previously SUN\) version of Java for all their Java needs. As of January 2019, the license is changing on Oracle Java which makes it no longer free for commercial use as well as the end of updates for Java 8. This has led to many people looking at alternatives to Oracle. Java itself is open source, so it will always be free and there are several other organizations offering their own builds of Java. If you want support, it matters which provider you get Java from.

This page is a work in progress to track the non-Oracle JREs and how to use them. Please send pull requests to this page with any additional information you have as this is a changing landscape right now.

You can read more about Oracle's changes in this post:

{% embed url="https://www.aspera.com/en/blog/oracle-will-charge-for-java-starting-in-2019/" caption="" %}

## Amazon Corretto

{% embed url="https://aws.amazon.com/blogs/opensource/amazon-corretto-no-cost-distribution-openjdk-long-term-support/" caption="" %}

Corretto is a build of OpenJDK maintained by Amazon. It is free and will have long term support. Initial tests show that Corretto 1.8 works with CommandBox and ACF 11.

## OpenJDK

{% embed url="https://openjdk.java.net/" caption="" %}

OpenJDK is Oracle's free version of Java. it comes with a 6 month support window. CommandBox has received a fair amount of testing on OpenJDK and everything seems to work.

## Azul Zulu

{% embed url="https://www.azul.com/" caption="" %}

Zulu is free and offers long term support. Zulu provides supported builds of OpenJDK. Initial tests show that Corretto 1.8 works with CommandBox and ACF 11.

## Installing on Windows

When running `box.exe` on Windows, the registry is used to determine the current versions of java that are installed. If you install a some non-Oracle JRE such as Corretto, you will not currently have the necessary registry entries created for box to find Java.

* **Oracle** - No manual action needed
* **Azul** - No manual action needed
* **OpenJDK** - Manual creation of registry keys required
* **Corretto** - Manual creation of registry keys required

You can manually create the needed keys by modifying and running the following registry entries. \(Contributed by Jim Pickering\)

{% code title="JavaSoft-Registry-Keys.reg" %}
```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\JavaSoft]

[HKEY_LOCAL_MACHINE\SOFTWARE\JavaSoft\Java Development Kit]
"CurrentVersion"="8.0.192"

[HKEY_LOCAL_MACHINE\SOFTWARE\JavaSoft\Java Development Kit\8.0.192]
"JavaHome"="C:\\Program Files\\Amazon Corretto\\jre8"
"RuntimeLib"="C:\\Program Files\\Amazon Corretto\\jre8\\bin\\server\\jvm.dll"
```
{% endcode %}

## Installing on \*nix

The `box` binary on \*nix uses your OS environment variables to locate Java. In the absense of an env var called `JAVA_HOME`, box will look for `java` in the default system path.

* **Oracle** - No manual action needed
* **Azul** - No manual action needed
* **OpenJDK** - Manual creation of `JAVA_HOME` required
* **Corretto** - Manual creation of `JAVA_HOME` required

To manually configure the `JAVA_HOME` env var on a \*nix system, edit your `/etc/profile` file to have these lines. Adjust the path as necessary based on your installation.

```bash
export JAVA_HOME=/opt/ibm/java-x86_64-60/
export PATH=$JAVA_HOME/bin:$PATH
```

## The JRE Folder

And as always, on any operating system and with any JRE provider, you can override what version of java is used by creating a folder called `JRE` in the same directory as the `box` or `box.exe` binary that contains the JRE you wish CommandBox to use. This will bypass all registry and env var checks above.

For macOS users who have installed CommandBox via HomeBrew, the installer creates a `box` _alias_ in `/usr/local/bin/` which points to the `box` _binary_ in the `/usr/local/Cellar/commandbox/<version>/bin/` directory. If you want CommandBox to use a particular version of the `JRE` then put the `jre` folder in the `/usr/local/bin/` directory. If you want CommandBox to have [a different home `.CommandBox` directory](https://commandbox.ortusbooks.com/setup/installation#homebrew-mac), place your `commandbox.properties` file in the `/usr/local/Cellar/commandbox/<version>/bin/` directory.

