# Non-Oracle JREs

In the past, 99% of people used the Oracle \(previously SUN\) version of Java for all their Java needs.  As of January 2019, the license is changing on Oracle Java which makes it no longer free for commercial use.  This has led to many people looking at alternatives to Oracle.  Java itself is open source, so it will always be free and there are several other organizations offering their own builds of Java.  If you want support, it matters which provider you get Java from.  

This page is a work in progress to track the non-Oracle JREs and how to use them.  Please send pull requests to this page with any additional information you have as this is a changing landscape right now.

## Amazon Corretto

{% embed url="https://aws.amazon.com/blogs/opensource/amazon-corretto-no-cost-distribution-openjdk-long-term-support/" %}

Corretto is a build of OpenJDK maintained by Amazon.  It is free and will have long term support.  Initial tests show that Corretto 1.8 works with CommandBox.

## OpenJDK

{% embed url="https://openjdk.java.net/" %}

OpenJDK is Oracle's free version of Java.  it comes with a 6 month support window.  CommandBox has received a fair amount of testing on OpenJDK and everything seems to work.

## Azul Zulu

{% embed url="https://www.azul.com/" %}

Zulu is free and offers long term support.  Zulu provides supported builds of OpenJDK.  CommandBox has not been tested yet on Zulu \(please contact us if you have tested it!\)

## Installing on Windows

When running `box.exe` on Windows, the registry is used to determine the current versions of java that are installed.  If you install a non-Oracle JRE such as Corretto, you will not currently have the necessary registry entries created for box to find Java.  You can manually create the needed keys by modifying and running the following registry entries.  \(Contributed by Jim Pickering\)

{% code-tabs %}
{% code-tabs-item title="JavaSoft-Registry-Keys.reg" %}
```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\JavaSoft]

[HKEY_LOCAL_MACHINE\SOFTWARE\JavaSoft\Java Runtime Environment]

[HKEY_LOCAL_MACHINE\SOFTWARE\JavaSoft\Java Runtime Environment\1.8]
"JavaHome"="C:\\Program Files\\Amazon Corretto\\jre8"
"MicroVersion"="0"
"RuntimeLib"="C:\\Program Files\\Amazon Corretto\\jre8\\bin\\server\\jvm.dll"

[HKEY_LOCAL_MACHINE\SOFTWARE\JavaSoft\Java Runtime Environment\1.8.0_192]
"JavaHome"="C:\\Program Files\\Amazon Corretto\\jre8"
"MicroVersion"="0"
"RuntimeLib"="C:\\Program Files\\Amazon Corretto\\jre8\\bin\\server\\jvm.dll"

[HKEY_LOCAL_MACHINE\SOFTWARE\JavaSoft\Java Runtime Environment\1.8.0_192\MSI]
"AUTOUPDATECHECK"="0"
"AUTOUPDATEDELAY"=""
"EULA"=""
"FROMVERSION"="NA"
"FROMVERSIONFULL"=""
"FullVersion"="1.8.0_192"
"INSTALLDIR"="C:\\Program Files\\Amazon Corretto\\jre8"
"JAVAUPDATE"="0"
"JU"=""
"OEMUPDATE"=""
"PRODUCTVERSION"="8.0.1920.12"


```
{% endcode-tabs-item %}
{% endcode-tabs %}



