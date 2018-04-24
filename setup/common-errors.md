# Common Errors

Here are some common issues starting up CommandBox and troubleshooting help.

## Could not load library. Reasons: \[no jansi in java.library.path ... Access is denied\]

If you have a Windows machine which has been locked down to not allow DLL files in the user's `appData` folder, you may receive a message similar to this when attempting to start CommandBox.

```text
Could not load library. Reasons: [no jansi in java.library.path, C:\Users\some.user\AppData\Local\Temp\1\jansi-64-9170657940034638384.dll: Access is denied]
        at org.fusesource.hawtjni.runtime.Library.doLoad(Library.java:182):182
        at org.fusesource.hawtjni.runtime.Library.load(Library.java:140):140
        at org.fusesource.jansi.internal.CLibrary.<clinit>(CLibrary.java:42):42
        at org.fusesource.jansi.AnsiConsole.wrapOutputStream(AnsiConsole.java:48):48
        at org.fusesource.jansi.AnsiConsole.<clinit>(AnsiConsole.java:38):38
        at jline.AnsiWindowsTerminal.detectAnsiSupport(AnsiWindowsTerminal.java:57):57
        at jline.AnsiWindowsTerminal.<init>(AnsiWindowsTerminal.java:27):27
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method):-2
        at sun.reflect.NativeConstructorAccessorImpl.newInstance(Unknown Source):-1
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(Unknown Source):-1
        at java.lang.reflect.Constructor.newInstance(Unknown Source):-1
        at java.lang.Class.newInstance(Unknown Source):-1
        at jline.TerminalFactory.getFlavor(TerminalFactory.java:211):211
        at jline.TerminalFactory.create(TerminalFactory.java:102):102
        at jline.TerminalFactory.get(TerminalFactory.java:186):186
        at jline.TerminalFactory.get(TerminalFactory.java:192):192
        at jline.console.ConsoleReader.<init>(ConsoleReader.java:243):243
        at jline.console.ConsoleReader.<init>(ConsoleReader.java:235):235
        at jline.console.ConsoleReader.<init>(ConsoleReader.java:227):227
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method):-2
        at sun.reflect.NativeConstructorAccessorImpl.newInstance(Unknown Source):-1
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(Unknown Source):-1
        at java.lang.reflect.Constructor.newInstance(Unknown Source):-1
        at lucee.runtime.reflection.pairs.ConstructorInstance.invoke(ConstructorInstance.java:52):52
        at lucee.runtime.reflection.Reflector.callConstructor(Reflector.java:809):809
        at lucee.runtime.java.JavaObject.init(JavaObject.java:295):295
        at lucee.runtime.java.JavaObject.call(JavaObject.java:222):222
        at lucee.runtime.java.JavaObject.call(JavaObject.java:259):259
        at lucee.runtime.util.VariableUtilImpl.callFunctionWithoutNamedValues(VariableUtilImpl.java:743):743
        at lucee.runtime.PageContextImpl.getFunction(PageContextImpl.java:1599):1599
        at system.util.readerfactory_cfc$cf.udfCall(/commandbox/system/util/ReaderFactory.cfc:38):38
```

If you don't have the option of changing the security controls on your PC, then you can try changing your Windows environment variables of `TMP` and `TEMP` to repoint to another folder which does not have this restriction.

## /usr/bin/box: 87: exec: java: not found

If you receive a message like the one above, which was taken from a Linux machine, when starting CommandBox, this means that you do not have Java installed. You can solve this in three ways: 1. Download the JRE-included CommandBox install which comes with a folder called `jre` 2. Download your own jre and place it in a folder called `jre` in the same folder as the `box` binary. 3. install Java onto your machine and ensure the correct `JAVA_HOME` and `JRE_HOME` environment variables are set.

