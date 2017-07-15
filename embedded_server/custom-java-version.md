# Custom Java Version

By Default, your servers start using the same version of Java that the CommandBox CLI is using.  For people needing to run Adobe ColdFusion 9, or who just want to do some testing on different JREs, you can point each of your servers at a custom JRE and CommandBox will use it when starting the server.

```
server set jvm.javaHome="C:\Program Files\Java\jdk1.8.0_25"
```

To set the default version of Java for all the servers you start on your machine, use the global config setting defaults.

```
config set server.defaults.jvm.javaHome="C:\Program Files\Java\jdk1.8.0_25"
```

