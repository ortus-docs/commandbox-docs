# What's New in 5.5.1

#### ModCFML Support

ModCFML is a popular standard for running a CF server behind IIS or Apache web server and allowing your virtual hosts configured in the web servers to control the web root of the CF server so you can run more than one site on a single CF instance.  CommandBox has historically only had a single web root per server, but now you can use a single CommandBox instance to power as many Adobe CF or Lucee Server sites as you like.  CommandBox's ModCFML just needs to be enabled and can work with BonCode, mod\_cfml or Nginx.

**server.json**

```
{
   "ModCFML":{
        "enable":"true",
        "sharedKey":"my-secret"
    }
}
```

Read more here: [https://commandbox.ortusbooks.com/embedded-server/modcfml-support](https://commandbox.ortusbooks.com/embedded-server/modcfml-support)

#### Log4j 1.x is gone!

CommandBox's core is now 100% free of 1.x versions of the Log4j library.  Note, if you start an older Lucee server or any Adobe ColdFusion server, they may still have Log4j 1.x bundled with them.

#### Library updates

Many of the java libraries have been updated

* **JRE**-bundled version ships with Java 11.0.15+10
* **Undertow** updated to 2.2.17.Final
* **Lucee** updated to 5.3.9.133
* **JGit** updated to 5.13.0.202109080827-r
* **JLine** updated to 3.21.0
* **Runwar** updated to 4.7.4
* **Log4j** updated to 2.17.1

#### Set Java System Props directly in server.json

Does what it says.  Looks like this:

**server.json**

```
{
  "jvm" : {
    "properties" : {
       "foo" : "bar baz"
       "java.awt.headless" : "true"
    }
}
```

Read More Here: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/ad-hoc-java-system-properties](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/ad-hoc-java-system-properties)

#### JVM Args can be an array

JVM args and Runwar args can now be set an array of strings which prevents you from needing to escape or quote anything.

**Old way** (still works)

```
{
  "jvm" : {
    "args" : "-XX:+UseG1GC -XX:-CreateMinidumpOnCrash --add-opens=java.base/java.net=ALL-UNNAMED"
  }
}
```

**New way**&#x20;

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

Read More Here: [https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/jvm-args#ad-hoc-jvm-args](https://commandbox.ortusbooks.com/embedded-server/configuring-your-server/jvm-args#ad-hoc-jvm-args)

#### Request Dumper

There is now a handy little handler you can put in your server rules to view a console dump of all the header data related to your request and response.

**server.json**

```
{
  "web" : {
    "rules" : [
      "regex('(.*).cfm') -> dump-request()"
    ]
  }
}
```

#### OpenJDK Downloads

The java API has been moved from AdoptOpenJDK to the [Eclipse Adoptium](https://adoptium.net/) project.  This is basically the same project, it just changed names.

#### assertFalse command <a href="#assertfalse" id="assertfalse"></a>

Opposite of **assertTrue**.  Returns a passing (0) or failing (1) exit code whether falsy parameter passed. Truthy values are "yes", "true" and positive integers. All other values are considered falsy

```
assertFalse false && echo "The inputs was false"
```

#### assertNotEqual command <a href="#assertnotequal" id="assertnotequal"></a>

Opposite of **assertEqual**.  Returns a passing (0) or failing (1) exit code whether both parameters DO NOT match. Comparison is case insensitive.

```
assertNotEqual "foo" "bar" && echo "The inputs are not equal"
```

#### Better command chaining

You can have more than two chained commands, and the command chain will keep executing so long as the next part is compatible with the previous exit code. Ex:

```
assertTrue false && echo "it was true!" || echo "back on track";
```

#### cfpm improvements

* We now set the JAVA\_HOME for the cfpm command if it doesn't exist
* An Adobe server no longer needs to be the default server for that web root
* If cfpm is run as part of a package or server script, it will default to the using the server currently doing something

#### box.exe finds Java better on Windows

Previous versions of Launch4j only used the Windows registry to find the installed version of java.  The following environment variables are now also checked in this order to attempt to find a JRE to use.

* JAVA\_HOME
* JRE\_HOME
* JDK\_HOME
* PATH

#### head Command

Does the same basic thing as the **tail** command, but it reads from the top of the file or input.

```
cat file.txt | head lines=10
```

#### X-Forwarded-For support disabled by default

For better security, CommandBox servers will not automatically obey X-Forwarded-For HTTP headers unless you enable it.&#x20;

```
server set web.useProxyForwardedIP=true
```

Only enable this setting if your CommandBox server is behind a trusted proxy which always sets this header.  Otherwise, a malicious client could spoof a trusted IP an bypass IP access control.

#### Relaxed semantic version parsing

Both of these commands will now do the same thing.  The second one used to error with version not found.

```
# technical correct
server start cfengine=lucee@5.3.9+133

# technical incorrect, but people still always tried it
server start cfengine=lucee@5.3.9.133
```

#### XML Love

XML formatting is now a first-class citizen of the print helper and the REPL.  You can pass a parsed XML doc and they will be formatted upon display.

```
❯ repl  'xmlParse(\'<root><user name="brad"/></root>\')'
<root>
  <user name="brad"/>
</root>
```

#### Mac Tray Icon Disabled

Due to lack of Java 9+ support in the Java library that creates tray icons, we've disabled the tray icon by default on MacOS.  If you want to brave the possibility of it not working or spilling errors in the console, you can re-enable it like so:

```
# for one server
server set trayEnable=true

# for all servers
config set server.defaults.trayEnable=true
```

Hopefully when the library gets updated, we'll be able to re-enable the tray icon by default.

### Release Notes

Here is the complete list of all tickets closed in this release
